import React from 'react';
import {
  StyleSheet,
  Text,
  View,
  Button,
  Vibration,
  TextInput,
} from 'react-native';

export default class App extends React.Component {
  constructor() {
    super();
    this.state = {
      second: 0,
      minute: 25,
      workMin: '25',
      workSec: '0',
      restMin: '5',
      restSec: '0',
      modeRest: false,
    };
  }
  timerId;

  shouldComponentUpdate() {
    if (this.state.second == 0) {
      this.setState(prevState => ({
        second: 59,
        minute: prevState.minute - 1,
      }));

      if (this.state.minute == 0) {
        Vibration.vibrate(500);
        if (this.state.modeRest) {
          this.setState({ modeRest: false });
          this.setState(prevState => ({
            second: this.state.workSec,
            minute: this.state.workMin,
          }));
        } else {
          this.setState({ modeRest: true });
          this.setState(prevState => ({
            second: this.state.restSec,
            minute: this.state.restMin,
          }));
        }
      }
    }
    return true;
  }

  timer = () => {
    this.setState(prevState => ({
      second: prevState.second - 1,
    }));
  };

  start = () => {
    this.stop();
    this.timerId = setInterval(this.timer, 1000);
  };

  stop = () => {
    clearInterval(this.timerId);
  };

  reset = () => {
    this.stop();
    if (this.state.modeRest) {
      this.setState({
        second: 0,
        minute: 5,
      });
    } else {
      this.setState({
        second: 0,
        minute: 25,
      });
    }
  };

  setRestTimeMin = time => {
    this.stop();
    if (this.state.modeRest) {
      this.setState({ minute: time });
      this.setState({ restMin: time });
    } else {
      this.setState({ restMin: time });
    }
  };

  setWorkTimeMin = time => {
    this.stop();
    if (this.state.modeRest) {
      this.setState({ workMin: time });
    } else {
      this.setState({ minute: time });
      this.setState({ workMin: time });
    }
  };

  setRestTimeSec = time => {
    this.stop();
    if (this.state.modeRest) {
      this.setState({ second: time });
      this.setState({ restSec: time });
    } else {
      this.setState({ restSec: time });
    }
  };

  setWorkTimeSec = time => {
    this.stop();
    if (this.state.modeRest) {
      this.setState({ workSec: time });
    } else {
      this.setState({ second: time });
      this.setState({ workSec: time });
    }
  };

  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.wt}>
          {this.state.modeRest ? 'REST TIMER' : 'WORK TIMER'}
        </Text>

        <Text style={styles.timer}>{`${this.state.minute} : ${
          this.state.second
        }`}</Text>

        <Text style={styles.button}>
          <Button title="start" onPress={this.start} />
          <Button title="stop" onPress={this.stop} />
          <Button title="reset" onPress={this.reset} />
        </Text>

        <Text style={styles.space}>
          <Text style={styles.time}>{'Work Time:'}</Text>
          <Text>{'   Mins: '}</Text>
          <TextInput
            value={this.state.workMin}
            style={styles.inp}
            placeholder="0"
            onChangeText={time => this.setWorkTimeMin(time)}
            keyboardType="numeric"
          />
          <Text>{'   Secs: '}</Text>
          <TextInput
            value={this.state.workSec}
            style={styles.inp}
            placeholder="0"
            onChangeText={time => this.setWorkTimeSec(time)}
            keyboardType="numeric"
          />
        </Text>

        <Text style={styles.space}>
          <Text style={styles.time}>{'Break Time:'}</Text>
          <Text>{'   Mins: '}</Text>
          <TextInput
            value={this.state.restMin}
            style={styles.inp}
            placeholder="0"
            onChangeText={time => this.setRestTimeMin(time)}
            keyboardType="numeric"
          />
          <Text>{'   Secs: '}</Text>
          <TextInput
            value={this.state.restSec}
            style={styles.inp}
            placeholder="0"
            onChangeText={time => this.setRestTimeSec(time)}
            keyboardType="numeric"
          />
        </Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },

  button: {
    padding: 10,
  },

  timer: {
    fontSize: 60,
  },

  wt: {
    fontSize: 40,
    fontWeight: 'bold',
  },

  inp: {
    width: 50,
    borderWidth: 1,
    borderColor: 'black',
    padding: 3,
  },

  time: {
    fontWeight: 'bold',
  },

  space: {
    margin: 5,
  },
});
