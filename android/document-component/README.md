

# JAAK IT - DocumentSdk

Este componente verifica la autenticidad de los documentos. La cámara de un dispositivo escanea el documento y devuelve una puntuación de la autenticidad del documento. Recomendamos el umbral de puntuación del documento falso <= 0,90 > documento auténtico.

### Características y requerimientos

A continuación se listan los requerimientos mínimos de desarrollo para la integración del componente al algún proyecto Android, tomar en cuenta que la configuración de la implementación debe coincidir con la del componente.

- compileSdkVersion: **32**
- minSdkVersion: **22**
- targetSdkVersion: **32**


### Instalación

DocumentSdk pude ser integrado a cualquier proyecto de la siguiente forma:

Agregue en **build.gradle(:app)**:

```
dependencies {

    implementation("com.jaak.document:document-sdk:1.0.0")

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

A continuación se muestran los pasos para inicializar el DocumentSdk, empezar a usar el componente y obtener los resultados.

### Inicialización

El SDK se inicializa de las siguientes maneras:

##### Por medio del token JWT
```
...

val documentSdk = DocumentSdk.Builder(activity)  
    .setListener(listener)  
    .isProduction(false)  
    .setJwtToken(jwtToken)  
    .build()

...
```

##### Por medio del API_KEY
```
...

val documentSdk = DocumentSdk.Builder(activity)  
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
|`listener`          |DocumentResultListener            |Interfaz que escucha los resultados del DocumentSdk            |
|`isProduction`          |Boolean|Para indicar si el ambiente es *Producción* (true) o *Desarrollo* (false)|
|`jwtToken`          |String|El token JWT de JAAK para autenticar al usuario|
|`apiKey`          |String|El ApiKey proporcionado por JAAK para autenticar al usuario|

### Uso

Una vez configurado el SDK puedes empezar el proceso de la siguiente forma:

```
...

button.setOnClickListener {  
  documentSdk.startComponent()  
}

...
```

### Resultados

Las respuestas del componente se pueden obtener de la implementación de la interfaz **DocumentResultListener**, cuyo contrato es el siguiente:

```
interface DocumentResultListener {

  fun onSuccess(document: Document)  

  fun onError(errorList: ErrorList? = null)  

  fun onCancel()
}
```

##### Donde:

|    Parametro            |Tipo                          |Descripción                         |
|----------------|-------------------------------|-----------------------------|
|`document`|Document            |El documento extraído de las imágenes capturadas      |
|`errorList`          |ErrorList            |La lista de errores que se reciban del proceso            |

# Clases

### Document
```
data class Document (  
    val eventId: String,  
    val status: Boolean,  
    val message: String,  
    val documentType: Int,  
    val documentData: DocumentData,  
    val documentMetadata: String,  
    val processingTime: Float
)
```
### DocumentData
```
data class DocumentData (
    val generalData: GeneralData,  
    val mechanicalReadingZone: String,  
    val specificData: List<Data>
)
```

### GeneralData
```
data class GeneralData (
    val name: String,  
    val surname: String,  
    val motherSurname: String,  
    val address: Address,  
    val validUntil: String,  
    val birthDate: String,  
    val gender: String
)
```

### Address
```
data class Address (
    val street: String,
    val neighborhood: String,
    val zipCode: String,
    val city: String,
    val state: String,
    val council: String
)
```

### Data
```
data class Data (
    val label: String,  
    val value: String
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


En este ejemplo se implementa en MainActivity la interfaz ***DocumentResultListener***, se inicializa el **DocumentSdk** y se inicia el componente usando la función ***startComponent()*** .

```
class MainActivity : AppCompatActivity(), DocumentResultListener {  
  
    private lateinit var binding: ActivityMainBinding  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
      super.onCreate(savedInstanceState)  
      binding = ActivityMainBinding.inflate(layoutInflater)  
      setContentView(binding.root)  

      val jwtToken = "your_jwt_here"  

      val documentSdk = DocumentSdk.Builder(this)  
          .setListener(this)  
          .isProduction(false)  
          .setJwtToken(jwtToken)  
          .build()  

      binding.btnOpenDocumentComponent.setOnClickListener {  
        documentSdk.startComponent()  
      }  
    }  
  
    override fun onSuccess(document: Document) {  
        Log.e("onSuccess", document.toString())  
    }  
  
    override fun onError(errorList: ErrorList?) {  
        Log.e("onError", errorList?.toString() ?: "error")  
    }  
  
    override fun onCancel() {  
        Log.e("onCancel", "process cancelled")  
    }  
}
```


