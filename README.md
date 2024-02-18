<!-- PROJECT LOGO -->
<div align="center">
   <a href="https://github.com/othneildrew/Best-README-Template">
      <img src="https://logodownload.org/wp-content/uploads/2014/12/estacio-logo-1-2048x1641.png" alt="estacio logo" width="80"                  height="80">
   </a>
    <h1 align="center"> Universidade Estácio de Sá </h1>
     <hr>
</div> 

* DESENVOLVIMENTO FULL STACK- 
* Disciplina: RPG0023  - Vamos criar um App.
* Semestre Letivo: 2024.1
* Repositorio Git: https://github.com/Gregdev22/Missao-1-Mundo-4

<hr>

* [EMERSON GREGORIO ALVES](https://github.com/Gregdev22) - MATRICULA: 2022.0908.4986
<hr>
 <h1 align="center"> Missão Prática | Nível 1 | Mundo 4 </h1>
 <h2 align="left" > Adicionando interatividade com o React Native. </h2> 
 <h3>Microatividade 1: Configurar o ambiente de desenvolvimento React Native </h3>
 <h3>Microatividade 2: Implementar a funcionalidade de entrada de texto em um componente React Native</h3>
 <h3>Microatividade 3: Implementar um Componente de Lista Dinâmica (ScrollView)</h3>
 <h3>Microatividade 4: Criando o visualizador de listas </h3>
 <h3>Microatividade 5: Empregar imagens, seja para exibir gráficos, ícones, fotos ou outros elementos visuais em um aplicativo React Native </h3>
 <hr>

 <h2> :clipboard: Objetivos da Prática </h2>

* Configurar o ambiente de desenvolvimento React Native.
* Implementar a funcionalidade de entrada de texto em um componente
React Native.
* Implementar um Componente de Lista Dinâmica (ScrollView);.
* Implementar componentes React Native para exibir informações de forma
dinâmica em listas;
* Empregar elementos visuais em um aplicativo React Native.
<hr>

<h2> Códigos </h2>

* App.js

``` Javascript
import logo from './logo.svg';
import './App.css';
import CatApp from './CatApp';
import Cadastro from './Cadastro';
import Lista from './Lista';
import Inicial from './Inicial';
import { NavigationContainer, DarkTheme } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';


const Stack = createStackNavigator();

function App() {
  return (
    //<Cat/>
    //<CatApp/>
    <NavigationContainer theme={DarkTheme}> 
      <Stack.Navigator initialRouteName="Inicial">
        <Stack.Screen name="Home" component={Inicial} />
        <Stack.Screen name="Cadastro" component={Cadastro} />
        <Stack.Screen name="Lista" component={Lista}/>
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```

* Cadastro.jsx

``` Javascript
import React, { useState } from 'react';
import { Text, Button, Image, TextInput, View, Platform, StyleSheet, Alert } from 'react-native';
import AsyncStorage from '@react-native-async-storage/async-storage';
import { createElement } from 'react';


const Cadastro = () => {

  const [alerta, setAlerta] = useState('');

  const [fornecedor, setFornecedor] = useState({
    nome: '',
    endereco: '',
    contato: '',
    categorias: '',
  });

  const [image, setImage] = useState(null);

  const handleInputChange = (field, value) => {
    setFornecedor({
      ...fornecedor,
      [field]: value,
    });
  };

  const selecionaImagem = Platform.OS === 'web' ? (event) => {
    setImage(URL.createObjectURL(event.target.files[0]));
  } : () => {
    ImagePicker.openPicker({
      width: 300,
      height: 400,
      cropping: true
    }).then(image => {
      setImage(image.path);
    });
  };

  const handleSubmit = async () => {
    try {
      const existingSuppliers = await AsyncStorage.getItem('@fornecedor');
      let newSuppliers = JSON.parse(existingSuppliers);
      if (!newSuppliers || !Array.isArray(newSuppliers)) {
        newSuppliers = [];
      }
      newSuppliers.push({ ...fornecedor, image });
      await AsyncStorage.setItem('@fornecedor', JSON.stringify(newSuppliers));
      setFornecedor({nome:'', 
      endereco: '',
      contato: '',
      categorias: ''});
      setAlerta('Usuario Cadastrado! ')
    } catch (e) {
      console.log(e);
    }
  };

  let ImagePicker;
  if (Platform.OS !== 'web') {
    ImagePicker = require('react-native-image-crop-picker').default;
  };

  return (
    <View style={styles.container}>
      <View>
        <Text style={styles.label}>Nome:</Text>
        <TextInput
          style={styles.input}
          value={fornecedor.nome}
          onChangeText={(value) => handleInputChange('nome', value)}
        />
      </View>
      <br />
      <View>
        <Text style={styles.label}>Endereço:</Text>
        <TextInput
          style={styles.input}
          value={fornecedor.endereco}
          onChangeText={(value) => handleInputChange('endereco', value)}
        />
      </View>
      <br />
      <View>
        <Text style={styles.label}>Contato:</Text>
        <TextInput
          style={styles.input}
          value={fornecedor.contato}
          onChangeText={(value) => handleInputChange('contato', value)}
        />
      </View>
      <br />
      <View>
        <Text style={styles.label}>Categorias:</Text>
        <TextInput
          style={styles.input}
          value={fornecedor.categorias}
          onChangeText={(value) => handleInputChange('categorias', value)}
        />
      </View>
      <br />
      {image && <Image source={{ uri: image }} style={styles.image} />}
      {Platform.OS === 'web' ? (
        <input type="file" accept="image/*" onChange={selecionaImagem} />
      ) : (
        <Button title="Selecionar Imagem" onPress={selecionaImagem} />
      )}
      <br />
      <Button title="Cadastrar" onPress={handleSubmit}  
      />
      <Text>{alerta}</Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 10,
    backgroundColor: '#F5FCFF',
  },
  label: {
    width: 100,
    marginRight: 10,
    fontSize: 16,
  },
  input: {
    flex: 1,
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    paddingLeft: 10,
  },
  image: {
    width: 20,
    height: 200,
    marginBottom: 20,
  },
});

export default Cadastro;
```
