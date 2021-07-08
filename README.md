# [bazelbuild/bazel#13142](https://github.com/bazelbuild/bazel/issues/13142): targetConfigured event with aborted payload 


Run this command:

```bash
# The `--@io_bazel_rules_go//go/config:race` flag is the main problem here.
bazel test --cache_test_results=no --@io_bazel_rules_go//go/config:race --build_event_json_file=bep.json //gotest/...
```

Then read `bep.json` with `less -S bep.json`. [A copy of bep.json is included in this repo](/bep.json).

The last line contains this:

```
{"id":{"targetConfigured":{"label":"@io_bazel_rules_go//go/config:race"}},"aborted":{},"lastMessage":true}
```

Which equal to the following protobuf `Aborted` message ([as per the protocol](https://github.com/bazelbuild/bazel/blob/f02c586e1f4e96572d3f32780e7d9fcb1af87f03/src/main/java/com/google/devtools/build/lib/buildeventstream/proto/build_event_stream.proto#L268-L315)):

```go
{
  Reason: AbortReasonUnknown,
  Description: "", // string
}
```
