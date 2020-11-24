# cvnet2-proto
A gRPC protocol for sending and configuring Eurorack Control Voltage via HTTP/2

# Introduction
CVNet2 is a protocol based on gRPC, which allows to transmit and receive control voltage, gate
signals and port configurations specifically made to control synthesizers via network.

As gRPC is supported by lots of languages, you can send and receive data for your synthesizer
from C++, Python, Go, TypeScript, JavaScript, etc.

## Implementation
```
service CV {
message CVMessage {
  uint32 channel = 1;
  float value = 2;

  google.protobuf.Timestamp Timestamp = 3;
}

message GateMessage {
  uint32 channel = 1;
  bool value = 2;
  float length = 3;

  google.protobuf.Timestamp Timestamp = 4;
}

message ConfigMessage {
  uint32 channel = 1;
  enum Mode {
    CV_IN = 0;
    CV_OUT = 1;
    GATE_IN = 2;
    GATE_OUT = 3;
  }
  Mode mode = 2;

  enum Range {
    ZERO_TO_FIVE = 0;
    ZERO_TO_TEN = 1;
    NEG_FIVE_TO_FIVE = 2;
  }
  Range range = 3;
}

service CV {
  rpc PinMode(ConfigMessage) returns (ConfigMessage){};

  rpc readCV(CVMessage) returns (CVMessage){};
  rpc writeCV(CVMessage) returns (CVMessage){};

  rpc readGate(GateMessage) returns (GateMessage){};
  rpc writeGate(GateMessage) returns (GateMessage){};

  rpc readCVStream(CVMessage) returns (stream CVMessage){};
  rpc writeCVStream(stream CVMessage) returns (CVMessage){};

  rpc readGateStream(GateMessage) returns (stream GateMessage){};
  rpc writeGateStream(stream GateMessage) returns (GateMessage){};
}
}
```
