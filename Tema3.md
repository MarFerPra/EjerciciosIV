## Tema 3: Ejercicios

#### 1. Darse de alta en algún servicio PaaS tal como Heroku, Nodejitsu, BlueMix u OpenShift.

Para realizar este hito me he dado de alta en Heroku (PaaS) y en mLab (MongoDB), con un plan gratuito en ambos casos.



#### 2. Crear una aplicación en OpenShift o en algún otro PaaS en el que se haya dado uno de alta. Realizar un despliegue de prueba usando alguno de los ejemplos.

Como aplicación de prueba se ha desplegado en Heroku la aplicación realizada en el hito anterior, [company-ranking](https://github.com/MarFerPra/company-ranking), lanzando también la base de datos de mongo en mLab.

#### 3. Realizar una app en express (o el lenguaje y marco elegido) que incluya variables como en el caso anterior.

En la aplicación anterior se han realizado rutas para manejar las entradas de la base de datos desde los componentes React del cliente, de forma que mediante llamadas ajax puedan crearse, destruirse o consultarse los distintos datos.

**Ejemplos:**

* Creación de usuario:
```
app.post('/api/users', function(req, res) {
    var user = new User();
    user.name = req.body.name;
    user.email = req.body.email;
    user.has_rated = null;

    user.save(function(error) {
      if (error) {
          res.json({
            text: "Error creating the user.",
            success: false
          });
      } else {
          res.json({
              text: "User created successfully.",
              success: true,
              model: user
          });
      }
    });
});
```

* Consulta de un usuario:
```
app.get('/api/users/:user_id', function(req, res) {
    var user_id = req.params.user_id
    User.findById(user_id, function(error, user) {
        if (error) {
            res.send(error);
        } else {
            res.json(user);
        }
    });
});
```

* Eliminación de un usuario:
```
app.delete('/api/users/:user_id', function(req, res) {
    if (req.params.userId) {
        User.remove({
            id: req.params.userId
        }, function(error, user) {
            if (error) {
                res.send(error);
            } else {
                res.json({
                    message: "User successfully deleted.",
                    userId: user.id
                });
            }
        })
    }
});
```

* Voto de una compañía por parte de un usuario:  
```  
app.post('/api/company/vote', function(req, res){
  if(req.body.company_id && req.body.user_id && req.body.score){
    Company.findById(req.body.company_id, function(error, company) {
      if(error){
        res.send({success: false, text: "Error finding the company."});
      } else {
        User.findById(req.body.user_id, function(error, user) {
          if(error){
            res.send({success: false, text: "Error finding the user."});
          } else {

            if(company.rated_by === null){
              company.rated_by = [];
            }

            company.rated_by.push(user._id);
            company.total_score = parseInt(req.body.score) + parseInt(company.total_score);

            if(user.has_rated === null){
              user.has_rated = [];
            }

            user.has_rated.push(company._id);

            var saved_company = true;
            company.save(function(error) {
              if (error)
                saved_company = false;
            });

            var saved_user = true;
            user.save(function(error) {
              if (error)
                saved_user = false;
            });

            if(saved_user && saved_company){
              res.send({success: true, text: "Successfully voted company."})
            } else {
              res.send({success: false, text: "Error saving models in db."});
            }

          }
        });
      }
    });
  } else {
      res.send({success: false, text: "Params error."});
  }
});
```

#### 4. Crear pruebas para las diferentes rutas de la aplicación.

Test unitario de la ruta que permite votar a una compañía:  
```
it("Vote a company", function(done) {
    var user = new User();
    user.name = 'Test user';
    user.email = 'Test@email.com';
    user.has_rated = [];

    user.save(function(error, user) {
        assert.notOk(error);
        var user_test_id = user._id;

        var company = new Company();
        company.name = "Test company";
        company.description = "Test description";
        company.total_score = 0;
        company.rated_by = [];

        company.save(function(error, company) {
            assert.notOk(error);
            var company_test_id = company._id;

            var data = {
                user_id: user_test_id,
                company_id: company_test_id,
                score: 10
            }

            request(app).post('/api/company/vote').send(data).expect(200).end(function(err, res) {
                assert.notOk(err);
                assert.strictEqual(res.body.success, true);
                done();

                Company.findById(company_test_id, function(error, company) {
                    assert.notOk(error);
                    User.findById(user_test_id, function(error, user) {
                        assert.notOk(error);

                        assert.strictEqual(company.total_score, 10);
                        var hasRated = (user.has_rated.indexOf(company_test_id) > -1);
                        var ratedBy = (company.rated_by.indexOf(user_test_id) > -1);

                        assert.ok(hasRated);
                        assert.ok(ratedBy);
                    });
                });
            });
        });
    });
});
```

#### 5. Instalar y echar a andar tu primera aplicación en Heroku.

La aplicación se ha desplegado correctamente en Heroku y la base de datos en mLab.
Tal y como se puede comprobar en el siguiente enlace:  https://company-ranking.herokuapp.com/

#### 6. Usar como base la aplicación de ejemplo de heroku y combinarla con la aplicación en node que se ha creado anteriormente. Probarla de forma local con foreman. Al final de cada modificación, los tests tendrán que funcionar correctamente; cuando se pasen los tests, se puede volver a desplegar en heroku.

#### 7. Haz alguna modificación a tu aplicación en node.js para Heroku, sin olvidar añadir los tests para la nueva funcionalidad, y configura el despliegue automático a Heroku usando Snap CI o alguno de los otros servicios, como Codeship, mencionados en StackOverflow.

Se ha añadido un link al historial de tests en TravisCI en la cabecera de la plataforma, así como la imagen indicativa del estado del ultimo test.  

En Heroku se ha configurado tanto el despliegue automático como el despliegue después de pasar los tests con la herramienta de integración continua:  

![Configuracion de heroku](http://i1268.photobucket.com/albums/jj576/marcofp0/heroku-travis_zpsekl1pt0y.png)

#### 8. Preparar la aplicación con la que se ha venido trabajando hasta este momento para ejecutarse en un PaaS, el que se haya elegido.

La aplicación preparada es [Teletorrent](https://github.com/MarFerPra/teletorrent), el proyecto principal de la asignatura.  
Por conveniencia, se ha desplegado también en Heroku y utilizando una base de datos externa en mLab.
