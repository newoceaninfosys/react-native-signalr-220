```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import '@nois/react-native-signalr';

import React, { Component } from 'react';
import {
  Platform,
  StyleSheet,
  Text,
  View,
  Alert
} from 'react-native';

const instructions = Platform.select({
  ios: 'Press Cmd+R to reload,\n' +
  'Cmd+D or shake for dev menu',
  android: 'Double tap R on your keyboard to reload,\n' +
  'Shake or press menu button for dev menu',
});

type Props = {};
export default class App extends Component<Props> {
  componentDidMount() {
    const connection = $.hubConnection('https://e98cf97a.ngrok.io',
      {
        logging: true,
        // qs: { Bearer: '...' }
      });

    const hub = connection.createHubProxy('messageHub');

    hub.on('chat', function (data) {
      Alert.alert('chat message', data);
    });

    connection.start(/*{ transport: 'longPolling' }*/ /*{ transport: ['webSockets', 'longPolling'] }*/)
      .done(function () {
        Alert.alert('Signalr has been connected!');
        // Wire up Send button to call NewContosoChatMessage on the server.
        // $('#newContosoChatMessage').click(function () {
        //   contosoChatHubProxy.invoke('newContosoChatMessage', $('#displayname').val(), $('#message').val());
        //   $('#message').val('').focus();
        // });
      });

    connection.connectionSlow(function () {
      Alert.alert('We are currently experiencing difficulties with the connection.')
    });

    connection.error(function (error) {
      Alert.alert('SignalR error: ' + error)
      console.log('[SIGNALR ERROR]', error);
    });
  }

  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
        <Text style={styles.instructions}>
          To get started, edit App.js
        </Text>
        <Text style={styles.instructions}>
          {instructions}
        </Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});

```
