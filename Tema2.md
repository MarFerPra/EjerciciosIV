## Tema 2: Ejercicios

#### 1. Instalar alguno de los entornos virtuales de node.js (o de cualquier otro lenguaje con el que se esté familiarizado) y, con ellos, instalar la última versión existente, la versión minor más actual de la 4.x y lo mismo para la 0.11 o alguna impar (de desarrollo).

* Instalación de NVM:  
![NVM](http://i1268.photobucket.com/albums/jj576/marcofp0/nvm-version_zpszydy9atm.png)  

* Instalación de las versiones de Node 4.6.0 (LTS) y 6.7.0 (Current):
![Node](http://i1268.photobucket.com/albums/jj576/marcofp0/node-versions_zpsopav6ul8.png)


#### 2. Como ejercicio, algo ligeramente diferente: una web para calificar las empresas en las que hacen prácticas los alumnos.

[Repositorio de la aplicación](https://github.com/MarFerPra/company-ranking)

Las acciones serían:  
* Crear empresa
* Listar calificaciones para cada empresa
* Crear calificación y añadirla (comprobando que la persona no la haya añadido ya)
* Borrar calificación (si se arrepiente o te denuncia la empresa o algo)
* Hacer un ránking de empresas por calificación, por ejemplo
* Crear un repositorio en GitHub para la librería y crear un pequeño programa que use algunas de sus funcionalidades.

Si se quiere hacer con cualquier otra aplicación, también es válido. Se trata de hacer una aplicación simple que se pueda hacer rápidamente con un generador de aplicaciones como los que incluyen diferentes marcos MVC. Si cuesta mucho trabajo, simplemente prepara una aplicación que puedas usar más adelante en el resto de los ejercicios.

#### 3. Ejecutar el programa en diferentes versiones del lenguaje. ¿Funciona en todas ellas?

Ejecución y test mediante Travis CI de la aplicación utilizando distintos sistemas operativos y versiones de Node.js:

![TravisCI](http://i1268.photobucket.com/albums/jj576/marcofp0/tests-travis_zps0pcebm8t.png)

#### 4. Crear una descripción del módulo usando package.json. En caso de que se trate de otro lenguaje, usar el método correspondiente.

El archivo package.json está incluído en el repositorio del proyecto, con el siguiente contenido:  

![Package-json](http://i1268.photobucket.com/albums/jj576/marcofp0/package-json_zpswlrrg1wc.png)




#### 5. Automatizar con grunt, `gulp` u otra herramienta de gestión de tareas en Node la generación de documentación de la librería que se cree usando `docco` u otro sistema similar de generación de documentatión. Previamente, por supuesto, habrá que documentar tal librería.

#### 6. Para la aplicación que se está haciendo, escribir una serie de aserciones y probar que efectivamente no fallan. Añadir tests para una nueva funcionalidad, probar que falla y escribir el código para que no lo haga (vamos, lo que viene siendo TDD).

Durante el desarrollo se han utilizado asserts para comprobar que las operaciones sobre la base de datos se realizaban correctamente. Han fallado especialmente al esperar que la función de búsqueda devolviese un valor, cuando realmente era necesario trabajar con el modelo devuelto dentro de un callback.  
No se muestran capturas del proceso ya que los asserts los he realizado durante el desarrollo de la aplicación y no tenía en mente escribirlos de cara al ejercicio.

En el punto siguiente se describe el test unitario implementado.

#### 7. Convertir los tests unitarios anteriores con assert a programas de test y ejecutarlos desde mocha, usando descripciones del test y del grupo de test de forma correcta. Si hasta ahora no has subido el código que has venido realizando a GitHub, es el momento de hacerlo, porque lo vas a necesitar un poco más adelante.

Al proyecto se ha añadido un test unitario para la ruta '/', comprobando que la aplicación se lanza correctamente y devuelve el contenido esperado por defecto.   
Y por último, y más importante, un test con la funcionalidad más compleja de la API, votar una empresa a partir de el id de la empresa, el id del usuario y una puntuación concreta.  

Además de lanzar los tests, se ha añadido Travis CI como herramienta de integración continua con github, de forma que se realizan tests para cada actualización del repositorio de forma automática.  

```
describe("Routes", function() {
  [...]
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


}

```
