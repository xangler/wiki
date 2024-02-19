# protoc使用

## 扩展类型
```protocol
import "google/protobuf/struct.proto";
import "google/protobuf/any.proto";
message Request{
    google.protobuf.Any data_a = 1;
    google.protobuf.Struct data_s = 2;
    map<string, string> data_m = 3;
    oneof data_o{
        string email = 4;
        string phone = 5;
    }
}
```

## 默认值与未设置
```protocol
import "google/protobuf/wrappers.proto";
import "google/protobuf/field_mask.proto";
message Request{
    int64 price = 1;
    google.protobuf.Int64Value price_w = 2; //使用wrapper
    optional int64 price_o = 3;             //3.15+使用optinal
    google.protobuf.FieldMask mask = 4;     //使用mask标记
}
```