# EJERCICIO 1: EXAMEN INGENIERO DE DATOS
Dara Eugenia Gama Sandoval
darae.gs@gmail.com
May, 2022

## Objectivo

Construir un proceso de ETL basado en el archivo:
data_prueba_tecnica.csv.
La salida es un dashboard de AmazonQuickSights que muestre los atributos *name* y *created_at* 
y un tercero que representan la suma diaria de las ventas de cada cliente: *day_ammount_sold*.

## Evaluación Costo-Beneficio

ETL: *Extract, Transform, Load*

La parte de extracción para este ejercicio es rápida al ya tener el dataset y haber seguido las instrucciones para subirlo a un bucket.

Para la  transformación se debe hacer una investigación para identificar la mejor herramienta  a usar. Las opciones provistas son VM y Sagemaker. Se agregan otras dos opciones que se encontraron al profundizar: Data Pipeline y Glue.

La carga de datos será como un dataset en AWS QuickSight.



* VM (EC2)
Es la opción más completa y más personalizable con menor presupuesto. 
Sin embargo presenta algunos inconvenientes que me hacen ponerla como última opción:
Primeramente por el tiempo de configuración. Aunque pueda no ser muy compleja puede ser tardaba ya sea por la velocidad de la red o el hecho de tener que  
entrar a documentación para instalar los paquetes, selección de recursos y  correr los comandos correctos; problamente me toparía con issues. Si tuviera experiencia probablemente tomaría esa opción.Cómo el tiempo es un factor en la evaluación para saltar ese paso y optaré por algún camino ya establecido por AWS. 
En cuánto a reproductibilidad,sería mucho mejor y más reproducible usar contenedores. (ECS)

[Costo:](https://aws.amazon.com/ec2/pricing/) Free Tier (750 horas de Linux y Windows, cada mes for un año a partir de la creación de la cuenta)

* Notebook Sagemaker
Ventaja: Hay bastantes ejemplos ya creados y es flexible, usa VM.

[Costo:](https://aws.amazon.com/sagemaker/pricing/) Free Tier (250 horas cada mes por 2 meses, a apartir de la creación del primer recurso de Sagemaker)

* AWS Data Pipeline
Ventaja: Flexibilidad y fácilidad
Le das el input, la transformación puede ser con lambda y fácilmente se puede conectar con QuickSight.

[Costo:](https://aws.amazon.com/codepipeline/pricing/) Free Tier (1 pipeline activo cada mes)


* [AWS Glue](https://aws.amazon.com/glue/?sc_channel=EL&sc_campaign=Demo_2018_vid&sc_medium=YouTube&sc_content=video2139&sc_detail=ANALYTICS&sc_country=US)
[Getting Started with AWS Glue ETL by Amazon Web Services](https://www.youtube.com/watch?v=z3HeHlWg88M)

El servicio Glue en general está más enfocado al proceso de juntar los datos de distintas fuentes y en pasar menos tiempo en el código. El *subservicio* AWS Glue DataBrew que da un profile de los datos que podríá ser útil para contestar las preguntas del examen.
Ventaja: Rapidez. Sugiere transformaciones basadas en los datos.

[Costo Data Brew:](https://aws.amazon.com/glue/pricing/) No hay free tier 1 USD por cada 30 minutos de uso.

* Lambda
Ventaja: Poco costo computacional.

[Costo:](https://aws.amazon.com/lambda/pricing/) No hay free tier (>= 0.0000000021 USD)


Todas las opciones tienes compatibilidad sencilla con QuickSights



### Conclusiones
Se plantearon dos opciones.
Plan A:
1. Hacer un análisis directamente en QuickSight de los datos crudos
2. Usar un notebook Sagemaker
3. Crear dashboard

Plan B:
1. Usar AWS Glue DataBrew para visualizar los datos
2. Pequeño códido para probablemente limpiar, hacer la suma y cualquier otra cosa encontrada en la visualización.
3. Crear un data pipeline o una función Lambda
5. Crear dashboard

Desventajas plan A: Sagemaker está más enfocado a una transformación más compleja y profunda.
Ventaja plan A: Sagemaker tiene free tier

Desventaja plan B: Glue DataBrew te cobra desde la primera hora de uso, al igual que el servicio Lambda.


Plan C:
1. Usar un notebook de Sagemaker
3. Crear un dashboard


La mejor oción ideal para un ambiente más de producción dónde constantemente se ocupa acceso al resultado sería crear una función Lambda que importe y exporte los datos del bucket. 
No seleccionaré esta opción pues solamente lo necesitaré para la resolución del ejercicio y el free tier de Sagemaker es más que suficiente.


## Crear notebook Sagemaker con Permission an Encryption: Execution Role.

Como primer acercamiento con QuickSights se subió el archivo de los datos crudos directamente a QuickSight como ejercicio prueba y QuickSights detectó los siguientes errores en los datos.

Transformaciones: 

10001 rows had more or fewer values than the number of columns.

2 rows where the number of bits in number representation is too large.

2 rows where amount field numeric values could not be interpreted.

3 rows where created_at field date values were not in a supported date format.

Siguiendo desde el punto 2.3 al punto 4.3 del [Tutorial base](https://amazon-sagemaker.com/es/mlops/prerrequisites/notebook/)
saltándose la creación de contenedores(aunque es muy útil para la reproductibilidad, llevaría más tiempo en indagar la forma correcta de hacerlo.Se deja dicha investigación al final.)

Subir el archivo data_transsformation.ipynb
