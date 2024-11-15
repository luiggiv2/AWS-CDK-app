# Creación de aplicación AWS CDK!

Utilizaremos el AWS Cloud Development Kit (AWS CDK) utilizando la AWS CDK Command Line Interface (AWS CDK CLI) para desarrollar la aplicación CDK.

## Requisitos previos

* AWS CLI instalado y configurado.
* Node.js 14.15.0 o posterior instalado
* Maven 3.9 o posterior instalado
* Netbeans (u otra IDE para java)
* Tener cuenta AWS profesional o académica

## Paso 1: Crea tu proyecto CDK

Desde un directorio de inicio de su elección, cree y navegue a un directorio llamado hello-cdk

```
mkdir hello-cdk && cd hello-cdk
```
**Note:** No olvide nombrar al directorio `hello-cdk` ya que este nombre de directorio será utilizado por la CLI CDK para nombrar cosas dentro del código CDK.

Una vez creado el directorio y dentro de él, se procede a iniciaizar un proyecto CDK con la siguiente línea de comando.

```java
cdk init app --language java
```

Luego procederemos a importar el proyecto en la IDE que mantengas instalada.

El comando `cdk init` crea una estructura de archivos y carpetas dentro del `hello-cdk`, Nos enfocaremos en los archivos:

**HelloCdkApp.java**
```java
package com.myorg;

import software.amazon.awscdk.App;
import software.amazon.awscdk.Environment;
import software.amazon.awscdk.StackProps;

import java.util.Arrays;

public class HelloCdkApp {
  public static void main(final String[] args) {
    App app = new App();

    new HelloCdkStack(app, "HelloCdkStack", StackProps.builder()
      .build());

    app.synth();
  }
}
```
**Y HelloCdkStack.java.** 

```java
package com.myorg;

import software.constructs.Construct;
import software.amazon.awscdk.Stack;
import software.amazon.awscdk.StackProps;

public class HelloCdkStack extends Stack {
  public HelloCdkStack(final Construct scope, final String id) {
    this(scope, id, null);
  }

  public HelloCdkStack(final Construct scope, final String id, final StackProps props) {
    super(scope, id, props);

    // Define your constructs here
  }
}
```

## Paso 2: Configure su entorno AWS

En este paso, configure el entorno AWS para su pila de CDK. Al hacer esto, especificas cuál entorno se implementará en la pila de CDK. Primero, determinanos el entorno de AWS que se desea usar. Un entorno de AWS consiste en una cuenta de AWS y Región de AWS.

Utilizaremos la CLI de AWS para obtener nuestro ID de la cuenta de AWS. Para ello ejecutamos el siguiente comando.

```
aws sts get-caller-identity --query "Account" --output text
```
Y obtendremos nuestra ID de nuestra cuenta AWS.

<img width="1026" alt="Captura de pantalla 2024-11-15 a la(s) 3 57 34 p m" src="https://github.com/user-attachments/assets/bbc90751-f1da-4886-8e32-3ad0bb465559">

También debemos obtener la región que se configuró para nuestro perfil, con el siguiente comando.

```
aws configure get region
```

<img width="606" alt="Captura de pantalla 2024-11-15 a la(s) 4 04 39 p m" src="https://github.com/user-attachments/assets/7c531684-591e-472e-b4e4-633e88251e02">

Una vez obtenido estos datos, procederemos a configurar el entorno AWS modificando el archivo `HelloCdkStack`.

```java
package com.myorg;

import software.amazon.awscdk.App;
import software.amazon.awscdk.Environment;
import software.amazon.awscdk.StackProps;

import java.util.Arrays;

public class HelloCdkApp {
    public static void main(final String[] args) {
        App app = new App();

        new HelloCdkStack(app, "HelloCdkStack", StackProps.builder()
                .env(Environment.builder()
                        .account("586410280628")  //ID cuenta AWS 
                        .region("us-east-1")      //Región configurada
                        .build())

                .build());

        app.synth();
    }
}
```

## Paso 3: Arranque su entorno AWS

En este paso al contar con una cuenta AWS Academy no contamos con ciertos privilegios, para ello vamos a realizar una pequeñas configuraciones. Para ello vamos a ejecutar el siguiente comando.

```
cdk bootstrap --show-template > bootstrap-template.yaml
```
En el cuál nos va a generar un archivo llamado `bootstrap-template.yaml`

<img width="362" alt="Captura de pantalla 2024-11-15 a la(s) 4 18 04 p m" src="https://github.com/user-attachments/assets/4545f5f5-89bf-4582-b78a-b4d0502c4fb5">

Vamos a abrir ese archivo con nuestra IDE que tengamos instalado y vamos a comentar desde la línea **272** hasta la **568**. Debería quedar algo así:

<img width="789" alt="Captura de pantalla 2024-11-15 a la(s) 4 27 35 p m" src="https://github.com/user-attachments/assets/d390990c-e4b1-4e93-b461-37f4ca553f31">

Luego en la línea **159**

<img width="643" alt="Captura de pantalla 2024-11-15 a la(s) 4 29 32 p m" src="https://github.com/user-attachments/assets/13e7c246-0e6b-488c-ad37-69c3684c1570">

Vamos a borrar y vamos a dejarla con `"*"`, debería quedar así:

<img width="460" alt="Captura de pantalla 2024-11-15 a la(s) 4 30 49 p m" src="https://github.com/user-attachments/assets/556b60ef-03d9-4904-96ff-63b44d73c741">

Posterior a eso, procedemos a crear el bootstrap ejecutando el siguiente comando hacia nuestro archivo `bootstrap-template.yaml`.

```
cdk bootstrap --template bootstrap-template.yaml
```

Y debería verse así en tu consola o terminal.

<img width="1086" alt="Captura de pantalla 2024-11-15 a la(s) 4 36 32 p m" src="https://github.com/user-attachments/assets/030accc9-e5c9-428c-9917-951c46b4a813">

Si accedemos a nuestra cuenta AWS, CloudFormation; veremos que la pila (stack) se ha creado con éxito. Debería verse algo así:

<img width="1541" alt="Captura de pantalla 2024-11-15 a la(s) 4 39 52 p m" src="https://github.com/user-attachments/assets/460a1f65-ec29-4717-ac1e-73041523d0e3">

## Paso 4: Construye tu aplicación CDK

Ahora procedemos a compilar con el siguiente comando.

```
mvn compile -q
```

Y deberíamos ver algo así:

<img width="1004" alt="Captura de pantalla 2024-11-15 a la(s) 4 46 17 p m" src="https://github.com/user-attachments/assets/243bd3ef-b6ae-4a08-bc0b-b06a95100531">

## Paso 5: Defina su función Lambda

En este paso, importamos aws_lambda desde la biblioteca de construcciones de AWS y utiliza la construcción Function L2. En este paso vamos a modificar el archivo `HelloCdkStack.java`.
* Definimos la función lambda, función asíncrona que devuelve una cadena que devuelve "Hello CDK!" en un json.

```java
package com.myorg;

import software.constructs.Construct;
import software.amazon.awscdk.Stack;
import software.amazon.awscdk.StackProps;
// Import Lambda function
import software.amazon.awscdk.services.lambda.Code;
import software.amazon.awscdk.services.lambda.Function;
import software.amazon.awscdk.services.lambda.Runtime;

public class HelloCdkStack extends Stack {
  public HelloCdkStack(final Construct scope, final String id) {
    this(scope, id, null);
  }

  public HelloCdkStack(final Construct scope, final String id, final StackProps props) {
    super(scope, id, props);

    // Define the Lambda function resource
    Function myFunction = Function.Builder.create(this, "HelloWorldFunction")
      .runtime(Runtime.NODEJS_20_X) // Provide any supported Node.js runtime
      .handler("index.handler")
      .code(Code.fromInline(
        "exports.handler = async function(event) {" +
        " return {" +
        " statusCode: 200," +
        " body: JSON.stringify('Hello CDK!')" +
        " };" +
        "};"))
      .build();

  }
}
```

## Paso 6: Defina su URL de función Lambda

Ahora vamos a definir la URL, se utiliza el método auxiliar `addFunctionUrl` de la construcción `Function` para definir una URL de función Lambda. Para generar el valor de esta URL en la implementación, se creará una salida de AWS CloudFormation mediante la construcción `CfnOutput`.

```java
package com.myorg;

// ...
// Import Lambda function URL
import software.amazon.awscdk.services.lambda.FunctionUrl;
import software.amazon.awscdk.services.lambda.FunctionUrlAuthType;
import software.amazon.awscdk.services.lambda.FunctionUrlOptions;
// Import CfnOutput
import software.amazon.awscdk.CfnOutput;

public class HelloCdkStack extends Stack {
  public HelloCdkStack(final Construct scope, final String id) {
    this(scope, id, null);
  }

  public HelloCdkStack(final Construct scope, final String id, final StackProps props) {
    super(scope, id, props);

    // Define the Lambda function resource
    // ...

    // Define the Lambda function URL resource
    FunctionUrl myFunctionUrl = myFunction.addFunctionUrl(FunctionUrlOptions.builder()
      .authType(FunctionUrlAuthType.NONE)
      .build());

    // Define a CloudFormation output for your URL
    CfnOutput.Builder.create(this, "myFunctionUrlOutput")
      .value(myFunctionUrl.getUrl())
      .build();
  }
}
```

## Paso 7: Sintetice una plantilla de CloudFormation

En este paso, se prepara para la implementación sintetizando una plantilla de CloudFormation con el comando `cdk synth` de la CLI de CDK. Este comando realiza una validación básica de su código de CDK, ejecuta su aplicación de CDK y genera una plantilla de CloudFormation a partir de su pila de CDK.

Antes de sintetizar una plantilla, puede compilar opcionalmente su aplicación para detectar errores de sintaxis y tipo. Para obtener instrucciones, consulte el Paso 4.

```
cdk synth
```

## Paso 8: Implementa tu pila (stack) CDK

En este paso utilizaremos el comando `cdk deployment` de la CLI de CDK para implementar la pila de CDK. Este comando recupera la plantilla de CloudFormation generada y la implementa a través de AWS CloudFormation, que aprovisiona los recursos como parte de una pila de CloudFormation.

En este paso iremos neuvamente a nuesta cuenta **AWS**, **Roles** y buscamos el rol que se llama **LabRole** es el que se usa en los laboratorios o en las cuentas de AWS Academy. Y nos copiamos el nombre completo del rol, este nombre es único para cada cuenta AWS por lo que deben revisar en su cuenta.

<img width="1294" alt="Captura de pantalla 2024-11-15 a la(s) 5 10 31 p m" src="https://github.com/user-attachments/assets/992e5662-b16c-4424-9271-ada28ad88aa6">

Antes de ejecutar debemos quemar nuestro usuario en la función lambda que definimos en el paso 5, modificaremos el archivo `HelloCdkStack.java`.

```
                .role(Role.fromRoleArn(this, "LabRole", "arn:aws:iam::586410280628:role/LabRole"))
                .build();
```

<img width="869" alt="Captura de pantalla 2024-11-15 a la(s) 5 29 28 p m" src="https://github.com/user-attachments/assets/df0207a2-eefd-459b-956e-425604ca7496">

Recuerden colocar su ID de sus cuentas AWS.

También debemos importar `iam.Role` que debemos realizar.

```
import software.amazon.awscdk.services.iam.Role;
```

Luego en ejecutamos el comando:

```
cdk deploy -r arn:aws:iam::586410280628:role/LabRole
```

<img width="1370" alt="Captura de pantalla 2024-11-15 a la(s) 5 15 00 p m" src="https://github.com/user-attachments/assets/63d097bf-f48b-403b-a1a1-555ca80ac114">

Una vez que lo ejecutamos lo ha creado en nuestra cuenta AWS y nos devuelve una URL de la salida de la función para poder acceder por la URL.

<img width="1241" alt="Captura de pantalla 2024-11-15 a la(s) 5 39 00 p m" src="https://github.com/user-attachments/assets/5f716b7e-62b2-4e66-9c7b-4a4e09a09610">

Si nos copiamos esa URL y nos vamos al navegador podremos acceder a la misma y nos devuelve Hello CDK!

<img width="875" alt="Captura de pantalla 2024-11-15 a la(s) 5 38 14 p m" src="https://github.com/user-attachments/assets/601ad0a9-678e-4592-8cbe-b3e8ff5553cf">


Enjoy!
