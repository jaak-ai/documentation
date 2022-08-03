# JAAK IT - LivenessSdk

Este componente permite hacer un Liveness sobre el video capturado de tal manera que podamos utilizar la respuesta BestFrame dentro de otro proceso que queramos integrar.

### Características y requerimientos

A continuación se listan los requerimientos mínimos de desarrollo para la integración del componente al algún proyecto Android, tomar en cuenta que la configuración de la implementación debe coincidir con la del componente.

- compileSdkVersion: **32**
- minSdkVersion: **22**
- targetSdkVersion: **32**


### Instalación

LivenessSdk pude ser integrado a cualquier proyecto de la siguiente forma:

Agregue en **build.gradle(:app)**:

```
dependencies {

    implementation("com.jaak.liveness:liveness-sdk:1.0.0")

}
```
Y en **settings.gradle**:

```

...

dependencyResolutionManagement {
	repositories {
		google()  
		mavenCentral()
		maven {  
		  url 'https://us-maven.pkg.dev/jaak-saas/jaak-public-android-repo-release'  
		}
	}
}

...
```


#### Versiones:

|    Versión            |Url                          
|----------------|-------------------------------
|`1.0.0`|https://us-maven.pkg.dev/jaak-saas/jaak-public-android-repo-release           
|`1.0.0-SNAPSHOT`          |https://us-maven.pkg.dev/jaak-saas/jaak-public-android-repo-snapshot            



# Guía de uso

A continuación se muestran los pasos para inicializar el LivenessSdk, empezar a usar el componente y obtener los resultados.

### Inicialización

El SDK se inicializa de las siguientes maneras:

##### Por medio del token JWT
```
...

val livenessSdk = LivenessSdk.Builder(activity)  
    .setListener(listener)  
    .isProduction(false)  
    .setJwtToken(jwtToken)  
    .build()

...
```

##### Por medio del API_KEY
```
...

val livenessSdk = LivenessSdk.Builder(activity)  
    .setListener(listener)  
    .isProduction(false)  
    .setApiKey(apiKey)  
    .build()

...
```

##### Donde:


|    Parametro            |Tipo                          |Descripción                         |
|----------------|-------------------------------|-----------------------------|
|`activity`|AppCompatActivity            |Contexto del Activity          |
|`listener`          |LivenessResultListener            |Interfaz que escucha los resultados del LivenessSdk            |
|`isProduction`          |Boolean|Para indicar si el ambiente es *Producción* (true) o *Desarrollo* (false)|
|`jwtToken`          |String|El token JWT de JAAK para autenticar al usuario|
|`apiKey`          |String|El ApiKey proporcionado por JAAK para autenticar al usuario|

### Uso

Una vez configurado el SDK puedes empezar el proceso de la siguiente forma:

```
...

button.setOnClickListener {  
  livenessSdk.startLiveness()  
}

...
```

### Resultados

Las respuestas del Liveness se pueden obtener de la implementación de la interfaz **LivenessResultListener**, cuyo contrato es el siguiente:

```
interface LivenessResultListener {

  fun onLoading()

  fun onSuccess(bestFrame: BestFrame)  

  fun onError(errorList: ErrorList? = null)  

  fun onCancel()
}
```

##### Donde:

|    Parametro            |Tipo                          |Descripción                         |
|----------------|-------------------------------|-----------------------------|
|`bestFrame`|BestFrame            |El best frame del Liveness      |
|`errorList`          |ErrorList            |La lista de errores que se reciban del proceso            |

# Clases

### BestFrame
```
data class BestFrame(  
    var eventId: String  
    var status: Boolean 
    var evaluation: Double  
    var faces_found: List<String>  
    var message: String  
    var process_time: Double  
    var best_frame: String 
    var threshold: Double
)
```

### ErrorList
```
data class ErrorList(  
    val errors: List<String>,
    val detail: List<ErrorDetail> 
)
```

### ErrorDetail
```
data class ErrorDetail(  
    var msg: String  
    var type: String
)
```

# Ejemplo


En este ejemplo se implementa en MainActivity la interfaz ***LivenessResultListener***, se inicializa el **LivenessSdk** y se inicia el componente usando la función ***startLiveness()*** .

```
class MainActivity : AppCompatActivity(), LivenessResultListener {  
  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
      super.onCreate(savedInstanceState)  
      binding = ActivityMainBinding.inflate(layoutInflater)  
      setContentView(binding.root)  

      val jwtToken = "your_jwt_here"  

      val livenessSdk = LivenessSdk.Builder(this)  
          .setListener(this)  
          .isProduction(false)  
          .setJwtToken(jwtToken)  
          .build()  

      binding.bntOpenLiveness.setOnClickListener {  
        livenessSdk.startLiveness()  
      }  
    }  
  
    override fun onSuccess(bestFrame: BestFrame) {  
        Log.e("onSuccess", bestFrame.toString())  
    }  
  
    override fun onError(errorList: ErrorList?) {  
        Log.e("onError", errorList?.toString() ?: "error")  
    }  
  
    override fun onCancel() {  
        Log.e("onCancel", "liveness cancelled")  
    }  
  
    override fun onLoading() {  
        Log.e("onLoading", "loading results...")  
    }  
}
```
