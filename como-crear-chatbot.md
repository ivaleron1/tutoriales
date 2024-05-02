# Creación de Chat Bot en 10 pasos


### Introducción

Este es un tutorial que pretende enseñar cómo realizar un **Chat Bot** mediante **React Native** y **Dialogflow**.
Creará su propia aplicación funcional que le permita mantener una conversación básica con el bot aplicada a una temática concreta.

En esta guía aprenderá a: 

Inicializar su proyecto de React Native.
Configurar el proyecto.
Preparar la interfaz visual del chat.
Entrenar al modelo de Dialogflow.
Conectar el modelo con el chat.

### Requisitos previos

Conocimientos de **Java Script y React Native**
**Nodejs** y **npm** 
**Android Studio** y un emulador ya descargado desde esta plataforma además de su API 
 

## Paso 1: Inicializar proyecto de React Native

Lo primero que haremos será ir a la línea de comandos y haremos lo siguiente:

Instalamos expo-cli de forma global, la cual es una herramienta que facilita el proceso de configuración del entorno, dependencias y la implementación en dispositivos móviles.

```npm install -g expo-cli```

Lo inicializamos

```npx create-expo-app mi-primer-chatbot```

Accedemos al directorio del proyecto que se nos ha generado 

```cd mi-primer-chatbot```

Instalamos las dependencias 

```npm install ```

Ejecutamos el proyecto

```npx expo start```

Seleccionamos el entorno donde queremos que se ejecute (en nuestro caso android) y ya podremos ver cómo se nos abre una vista inicial en el dispositivo.

![image](https://github.com/ivaleron1/tutoriales/assets/165900790/b167608c-f8c7-40c8-aa9d-90b1dae7c016)


## Paso 2: Configurar el proyecto 

Ya tenemos la aplicación en ejecución así que procederemos con la instalación de los paquetes necesarios que usaremos en este caso. Para no tener que realizar demasiadas instalaciones, nos vamos a dirigir al archivo ```package.json``` que se encuentra en la raiz del proyecto 

```
"dependencies": {
    "expo": "~50.0.17",
    "expo-status-bar": "~1.11.1",
    "react": "18.2.0",
    "react-native": "0.73.6",
    "react-native-dialog": "^9.3.0",
    "react-native-dialogflow": "^3.2.2",
    "react-native-elements": "^3.4.3",
    "react-native-gesture-handler": "~2.9.0",
    "react-native-gifted-chat": "^2.4.0",
    "react-native-safe-area-context": "4.5.0"
  },
```
Una vez hagamos hecho eso ejecutamos el comando ```npm install``` de nuevo para que se descarguen las dependencias necesarias

## Paso 3: Preparar la interfaz visual del chat

El punto de entrada de la aplicación se encuentra en el archivo ```App.js```.
Una vez estemos dentro vemos que tenemos el mismo texto que aparece en el emulador dentro de las etiquetas View y Text.
Para no escribir el componente directamente en este lugar, nos creamos otro archivo con el nombre ```ChatBot.jsx``` e importamos las siguientes librerías en la parte superior que son las que vamos a usar

```
import React, { useState, useEffect } from 'react';
import { Text, View } from 'react-native';
import { SafeAreaView } from 'react-native-safe-area-context';
import { GiftedChat, Bubble } from 'react-native-gifted-chat';
import { Dialogflow_V2 } from 'react-native-dialogflow';
import { ImageBackground } from "react-native";
import { Image, Card, Button } from "react-native-elements";
import { TouchableOpacity } from 'react-native-gesture-handler';
import { Feather, FontsAwesome } from "@expo/vector-icons"
import { Alert } from 'react-native';
import { v4 as uuidv4 } from 'uuid';
import { ScrollView } from 'react-native-gesture-handler';
```

Definimos una constante con el nombre del componente cómo haríamos en cualquier otro proyecto de React native y lo exportamos para poder usarlo en el ```App.js```

```

const ChatBot = () => {

  
};

export default ChatBot;
```

Antes de continuar vamos a añadir el componente al archivo principal, importándolo primero y después añadiendo su etiqueta

```
import ChatBot from './ChatBot';

export default function App() {
  return (
    <View style={styles.container}>
      <ChatBot/>
    </View>
  );
}
```
Ahora regresamos a nuestro componente y agregamos en la parte que va a renderizar el componente "GiftedChat" con las siguientes propiedades básicas
El onSend se va a ejecutar cada vez que le demos al botón de enviar y también le indicamos el id del usuario principal de dicho chat.

```
return (
   
      <GiftedChat
       
        onSend={(message) => onSend(message)}
        user={{ _id: 1 }}
      
      />

    );
```

Los mensajes se van a almacenar en la propiedad mensajes de este componente, así que se la añadimos 

```
messages={messages}
```

Por ahora esa variable no está definida por lo que para poder usarla vamos a la parte superior del componente antes de la instrucción return y agregamos la siguiente constante. Como estamos en react y queremos que el estado se actualice correctamente en el componente cuando lo necesitemos así que lo implementaremos mediante el "hook" **useState()**. Le damos un estado inicial del array con el objeto que hace referencia al primer mensaje.

```
const [messages, setMessages] = useState([   
  {
    _id: 2,
    text: 'Bienvenido al Chat Bot', createdAt: new Date(),
    user: BOT },
  ]);
```

Lo que va a ocurrir es que no tenemos definido aún al objeto del usuario bot. Lo definimos también en la parte superior. 

```
const BOT = {
        _id: 2,
        name: 'Bot',
        avatar: botAvatar,
      };

```
Además para indicarle la imagen vamos a crear otra constante con el nombre que le dimos en el código anterior que contendrá el recurso de la imagen que deberemos almacenar en otro directorio del proyecto (en nuestor caso, dentro de la carpeta "assets")

```
const botAvatar = require('../../../assets/botAvatar.png');

```
Para personalizar y mejorar la interfaz visual podemos añadirle vistas y estilos cómo por ejemplo los siguientes: 

```
<SafeAreaView style={{
        flexDirection: "column",
        justifyContent: "space-between",
        
        backgroundColor: "white",
      
        height: "100%",
        width: "100%",
        borderColor: "black",
        borderRadius: 30,
        position: "relative",
       
        borderColor: "lightgray",
        
      
    }}>
    
    <ImageBackground
    resizeMode='cover'
    borderRadius={70}
    imageStyle={{ opacity: 0.2 }}
    source={{ uri: 'https://www.shutterstock.com/image-vector/social-media-sketch-vector-seamless-600nw-1660950727.jpg' }}
    style={{ flex: 1 }}>
        <View style={{
            flexDirection: "row",
            alignItems: "center",
            backgroundColor:"#C3CFC1",
            width: "90%",
            justifyContent: "space-around",
            borderRadius: 40,
            height: 70,
            position:"relative",
            alignSelf: "center"
            
        }}>
                       
          

            <View style={{flexDirection: "row", width: "45%", }}>
               
                <Image
                    source={{uri:("https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRWjavywBxDmpGIJIpVYOZHslYJxvZVUZnfNwMxBRLAog&s")}}
                    resizeMode='contain'
                    style={{
                        height:40,
                        width: 40,
                        borderRadius: 999,
                        
                    }}
                />    
                
                <View style={{
                        position: "absolute",                                
                        width: 10,
                        height: 10,
                        borderRadius: 5,
                        backgroundColor: "purple",
                        zIndex: 102,
                        borderWidth: 999,
                        borderWidth: 2,
                        borderColor: "white",                               
                        bottom: 8,
                        left: 30
                    }}>
                    
                </View>

                <View style={{
                    marginLeft: 16,
                  
                }}>
                    <Text style={{
                        fontSize: 20,
                        color: "black",
                        fontFamily: "sans-serif"
                    }}>
                        Bot
                    </Text>
                    <View style={{}}>
                        <Text style={{
                            fontSize: 12,
                            fontFamily: "regular",
                            color: "purple"
                        }}>
                            Online
                        </Text>

                    </View>
                   
                </View>
            </View>

            <View style={{
                flexDirection:"row",
                alignItems:"center"
            }}>
               

               
            </View>
            <TouchableOpacity>
                    <Feather
                        name="more-vertical"
                        size={24}
                        color="gray"
                    />
                        
            </TouchableOpacity>
        </View>
   
    <View style={{ flex: 1, backgroundColor: 'rgba(255,255,255,0.5)' }}>
      <GiftedChat
        messages={messages}
        onSend={(message) => onSend(message)}
       
        user={{ _id: 1 }}
      
      />
    </View>
  </ImageBackground>
  </SafeAreaView>
 
```


## Paso 3: Entrenar al modelo de Dialogflow 

Vamos a entrenar al modelo de Dialogflow que va a ser la plataforma que procese el input del usuario, lo procese y nos devuelva una respuesta adaptada a nuestras necesidades que ya en el próximo punto seremos capaces de conectar con la app de React Native.

### 1-Registro

Creamos una cuenta de google con la que vamos a iniciar sesión y en la que tendremos la key de la api almacenada. Una vez la tengamos nos registramos en Dialogflow y ya podríamos empezar a crear nuestros agentes.

### 2-Creación de agente

Cómo vamos a hacer un Chat Bot bastante sencillo con lo básico no nos hará falta más que un agente. Para crearlo simplemente le damos al menú que nos lo indica y ya podemos continuar,

### 3-Definición de intentos

Para que el chat bot nos entienda debemos indicarle una serie de preguntas las cuales se corresponderán con unas determinadas respuestas. Todo ello lo definimos en la interfaz que nos aparece al darle a crear. Ignoramos en principio el resto de utilidades y características que se nos ofrece porque no las necesitaremos todavía.

### 4-Entidades

Las entidades son una especie de etiquetas que se les asignan a las palabras y que el motor de lenguaje con la inteligencia articifial que tiene implantada debe intentar reconocer. Ya existen muchas por defecto pero si queremos que reconoza términos concretos como un tipo de materia debemos indicárselo creando una nueva entidad. En nuestro caso vamos a crear las entidades **hornos** y **materiales**.

#### 5-Contextos.

Cuando vayamos a crear el siguiente intento, si sabemos que sólo queremos acceder a él cuando hayamos iniciado el intento anterior entonces creamos un intento que descienda del otro y automáticamente le va a asignar como contexto de input, el contexto de output que tenga el primero.

### 6-Parámetros

También va a ser necesario saber que los parámetros normalmente son entidades que ha reconocido anteriormente y que pueden ser requeridas o no, además de poder darle un nombre o incluso crear parámetros personalizados que usaremos, por ejemplo, como **flags**, es decir, indicadores adicionales a modo de metadata para saber a que intento pertenecen y demás. 



## Paso 4: Conectar el modelo con el chat. 

Este va a ser el paso más largo ya que debemos obtener la clave de la api de dialogflow y toda la lógica que nos falta en React Native.

### 1-Crear API-KEY
Accedemos a google cloud con nuestra cuenta y generamos una api key seleccionando el proyecto correspondiente. Una vez la tengamos copiamos lo siguiente en un nuevo archivo llamado **.env.js** dentro de una constante que vamos a exportar a nuestro componente ChatBot


```
export const dialogflowConfig = {

    "type": "*******",
  
    "project_id": "*******",
  
    "private_key_id": "*******",
  
    "private_key": "*******",
  
    "client_email": "*******",
  
    "client_id": "*******",
  
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  
    "token_uri": "https://oauth2.googleapis.com/token",
  
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  
    "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/*******.gserviceaccount.com",
  
    "universe_domain": "googleapis.com"
  
  }
```

### 2-Importación de la configuración
```
import { dialogflowConfig } from '../../../env';


```



### 3-Hook para cargar la conexión con Dialogflow

```
  useEffect(() => {
    Dialogflow_V2.setConfiguration(
      dialogflowConfig.client_email,
      dialogflowConfig.private_key,
      Dialogflow_V2.LANG_SPANISH,
      dialogflowConfig.project_id,
    );
  }, []);

```

### 4-Definición de método para el envío de mensajes

Definimos ahora el método **onSend()** que recibe un array de mensajes por parámetros. Llamamos al método del hook **setMessages** el cual recibe los mensajes anteriores (esto es algo que hace el hook internamente) y en el cuerpo del método llamamos al método append de GiftedChat para pasarle en el primer argumento los mensajes anteriores y en el segundo los mensages que recibimos por parámetros del "onSend".

Almacenamos en una variable el mensaje que obtenemos (en este caso la posición 0 porque únicamente vamos a enviar un mensaje a la vez en el array)

Además llamamos al método **requestQuery** del objeto que hace referencia a Dialogflow al que le pasamos por parámetros el mensaje en primer lugar, y como segundo parámetro dos funciones callback que no hemos definido todavía. La primera de ellas es lo que va a hacer con el resultado que nos devuelva la API (en formato **JSON**) y la segunda el manejo de errores, que cómo no queremos complicarlo mucho, realizamos un console.log() del error.
```
const onSend = (messages = []) => {
        setMessages((previousMessages) => GiftedChat.append(previousMessages, messages));
    
        let message = messages[0].text;
    
        Dialogflow_V2.requestQuery(
          message,
          (result) => handleGoogleResponse(result),
          (error) => console.log(error),
        );
      };
```

### 5-Añadimos la propiedad "onQuickReply"

En el componente GiftedChat agregamos esta nueva propiedad que nos va a permitir que el bot responda al recibir el input 
```
onQuickReply={(quickReply) => onQuickReply(quickReply)}

```

Ahora vamos a definirla. 
Primero Le agregamos la respuesa al chat mediante el método que vimos en el punto anterior, después obtenemos el value de la posición 0 del array que se encuentra en la propiedad y le indicamos a Dialogflow de nuevo lo que debe hacer con dicha respuesta 

```

  const onQuickReply = (quickReply) => {
    setMessages((previousMessages) => GiftedChat.append(previousMessages, quickReply));

    let message = quickReply[0].value;

    Dialogflow_V2.requestQuery(
      message,
      (result) => handleGoogleResponse(result),
      (error) => console.log(error),
    );
  };
```
### 6-Definición del método "handleGoogleResponse()"

Este método como hemos dicho anteriormente se encarga de hacer algo cuando recibimos la respuesta de la API.
Por un lado guardamos en una variable denominada "texto" en este caso la respuesta que se encuentra en el objeto result el cual vino en estructura JSON desde la API. Dentro de este objeto accedemos a queryResult -> fulfillementMessages[0] -> text -> text[0].
 Ya tenemos 

```
  const handleGoogleResponse = (result) => {
    let text = result.queryResult.fulfillmentMessages[0].text.text[0];

**Obtención de parámetros**

Cómo dijimos anteriormente vamos a recibir una serie de parámetros junto con la respuesta. Para que se guarden correctamente me creo un hook para cada uno y que de esta forma cada vez que queramos actualizar su estado se haga correctamente. (Los inicializamos a null en el argumento de la función a la que le aplicamos la desestructuración)

```
    const [horno, setHorno] = useState(null);
    const [material, setMaterial] = useState(null);
    const [cantidad, setCantidad] = useState(0);
```

-A continuación declaramos variables locales del método "handleGoogleResponse()" para trabajar con los valores antes de activarlos en el hook.
```
   let hornoValue = null;
    let mensajeCalcularBiochar = null
    let mensajeCalcularBiocharDosis = null
```
Para saber validar que el parámetro al que intentamos acceder es el que realmente queremos ponemos una validación primero y de esta forma evitamos errores. Si nos fijamos en el siguiente código ahora en lugar de acceder a la propiedad de antes en el JSON, vamos a acceder a **parameters** el cual es un array asociativo cuyas claves son los nombres de los parámetros. (En el caso de unit-weight es una entidad que se envía como un objeto y accedemos a su propiedad amount que es el entero que nos indica la cantidad).

```

    //obtencion de horno
    if (result.queryResult.parameters) {

      if (result.queryResult.parameters['horno']){
        
        let hornoValue = result.queryResult.parameters['horno']
      
        if (hornoValue) {
          setHorno(hornoValue);         
        }     

      }
           
      
      //obtención de cantidad y material 
      if (result.queryResult.parameters['unit-weight'] && result.queryResult.parameters['material']) {
          
        let cantidadEnKilos = result.queryResult.parameters['unit-weight'].amount;
        setCantidad((prevCantidad) => {
          return cantidadEnKilos;
        });
      
        let material = result.queryResult.parameters['material'];
        setMaterial(material);

        if (result.queryResult.parameters['calcular']){
          mensajeCalcularBiochar = handleCalculateBiochar(horno, material, cantidadEnKilos);
        }
        
        
      }

```

    //obtencion de horno
    if (result.queryResult.parameters) {

      if (result.queryResult.parameters['horno']){
        
        let hornoValue = result.queryResult.parameters['horno']
      
        if (hornoValue) {
          setHorno(hornoValue);         
        }     

      }
```


En este caso particular nos ha interesado saber si el intento era el inicial para responder con una determinada explicación, para ello hemos creado un parámetro adicional en el intento inicial de Dialowflow que se denominara "inicio_calculo_biochar". Al ver que nos llega quiere decir que nos encontramos en el principio y que por lo tanto podemos enviar el mensaje de explicación. 
Para enviar el mensaje, llamamos al método sendBotResponse (no definido aún), le pasamos la información que queremos que se renderice en el mensaje y la imagen en caso de tenerla.
Tenemos condiciones adicionales para que en caso de que no se cumpla ninguna de las anteriores enviamos directamente la respuesta del bot. 

```
//en caso de que sea el inicio de la pregunta se nos muestra una imagen y un texto explicativo de qué es el biochar

      if(result.queryResult.parameters['inicio_calculo_biochar']){
        let biocharInfo = "El biochar o biocarbón es un producto que se obtiene tras un proceso de pirólisis (Ta entre 350 ºC y 800 ºC en ausencia de O2). Entre sus propiedades destacan su alto contenido en carbono (C) estable o recalcitrante, bajo contenido de nitrógeno, pH alcalino, baja densidad, alta porosidad y gran superficie específica."
        
       let img ='https://vercochar.innomakers.es/wp-content/uploads/2024/01/PO4-uai-2160x1440.png'
        
        sendBotResponse(biocharInfo, img)
        sendBotResponse(text)
        return true
      }
      

      //en el caso de que se reciba el parámetro "mostrar_tecnicas" mostramos las técnicas que hay con sus respectivas fotos como un menú de opciones para elegir
      if(result.queryResult.parameters['mostrar_tecnicas']){
        
        sendBotResponse(text)

        let msg= "Elige la técnica"
        let data = [
          {
            title: 'Compost',
            image: 'https://vercochar.innomakers.es/wp-content/uploads/2024/01/P02-1536x864.png',       
          },
          
          {
            title: 'Vermicompost',
            image: 'https://vercochar.innomakers.es/wp-content/uploads/2024/01/P03-1536x864.png',           
          },
          {
            title: 'Biochar',
            image: 'https://vercochar.innomakers.es/wp-content/uploads/2024/01/PO4-1536x864.png',           
          },
        ]
        
        sendBotResponse(msg, null, data)
        return true
      }
  
      if(mensajeCalcularBiocharDosis){
        sendBotResponse(text);
        sendBotResponse(mensajeCalcularBiocharDosis);
        return
      }
      if (mensajeCalcularBiochar) {
        sendBotResponse(text);
        sendBotResponse(mensajeCalcularBiochar);
        return 
      } 
        
      sendBotResponse(text);

     
     
    };
```

### 7-Definición del método "sendBotResponse()"

En este método nos encargamos de renderizar el texto en el <GiftedChat/> que nos llega por parámetros además de una posible imagen.
Tenemos el caso de que se nos envíe únicamente el texto, que lo incluimos en el objeto mensaje que definimos, el caso de que nos llegue una imagen la cual se la agregamos al objeto y el caso de que nos lleguen datos adicionales como sería el caso de que queramos añadir varias opciones y presentárselas al usuario.
Finalmente agregamos los mensajes al "GiftedChat"


```
const sendBotResponse = (text, img, data) => {
    let msg = {
      _id: uuidv4(),
      text,
      createdAt: new Date(),
      user: BOT,
  
    };
  
    if (img) {
    
     msg.image = img;
    }
  
    if (data) {
      msg.isOptions = true;
      msg.data = data
    }
  
    setMessages((previousMessages) => GiftedChat.append(previousMessages, [msg]));
  };
```

### 8-Definición del método "Personalización de las burbujas de mensaje"

Ya tenemos parte de la lógica báscia lograda, así que vamos a añadir lógica personalizar la presentación visual de los mensajes. Para ello agregamos la propiedad **renderBubble** al "GiftedChat" y le pasamos la referencia de la función que vamos a definir a continuación.

```
  renderBubble={this.renderBubble}
```
Definimos la función **renderBubble()** y le pasamos las props que pueda tener (aunque no hayamos definido "props" esta va a contener propiedades que le pase el componente automaticamente, ya que lo que le pasamos en la prop del GiftedChat es la referencia a la función para que internamente trabaje con ella).
Lo que hacemos después es aplicarle estilos y mediante el **spread operator** (...) para que las trate individualmente.



```
renderBubble = props => {

    return (
    <Bubble 
    {...props}
    textStyle = {{right: {color: "white"}}}
    wrapperStyle = {{
      left: {backgroundColor: "#C3CFC1"},
      right: {backgroundColor: "rgba(56, 96, 51,0.9)"},
    }}
    
    />)
  }

```


## CONCLUSIÓN

Ya hemos conseguido tener un chat bot funcional con lo básico al que le podremos agregar más cosas proximamente. Aquí tenemos el resultado. 


![image](https://github.com/ivaleron1/tutoriales/assets/165900790/53db287f-f567-417a-8470-f4e9f6815cc9)
