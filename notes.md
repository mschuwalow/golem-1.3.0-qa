# Golem-1.3.0 QA Notes

Creating a typescript should npm install automatically, so `golem app build` works immediately

---

Lots of moonbit warnings with 0.6.20 when running golem app build
```
/tmp/.tmpk12ngs/interface/golem/rpc/types/top.mbt:577:55-577:69 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/interface/golem/rpc/types/top.mbt:1995:67-1995:81 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/interface/golem/rpc/types/top.mbt:2789:43-2789:57 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/interface/golem/agent/common/top.mbt:48:50-48:61 [E0035] Warning: The word `constructor` is reserved for possible future use. Please consider using another name.
/tmp/.tmpk12ngs/interface/golem/agent/common/top.mbt:52:49-52:60 [E0035] Warning: The word `constructor` is reserved for possible future use. Please consider using another name.
/tmp/.tmpk12ngs/interface/golem/agent/guest/top.mbt:897:74-897:88 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/interface/golem/agent/guest/top.mbt:2011:92-2011:106 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/interface/golem/agent/guest/top.mbt:2270:92-2270:106 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/interface/golem/agent/guest/top.mbt:2552:74-2552:88 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/gen/interface/golem_qa/weather/weatherAgent/stub.mbt:9:28-9:77 [E0012] Warning: Unreachable code
/tmp/.tmpk12ngs/gen/interface/golem_qa/weather/assistantAgent/stub.mbt:6:28-6:77 [E0012] Warning: Unreachable code
/tmp/.tmpk12ngs/gen/interface/golem_qa/weather/assistantAgent/stub.mbt:23:28-23:77 [E0012] Warning: Unreachable code
/tmp/.tmpk12ngs/gen/gen_interface_golem_agent_guest_export.mbt:176:80-176:94 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/gen/gen_interface_golem_agent_guest_export.mbt:435:80-435:94 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/gen/gen_interface_golem_agent_guest_export.mbt:1431:80-1431:94 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmpk12ngs/gen/gen_interface_golem_agent_guest_export.mbt:1690:80-1690:94 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
```

---

Formatting of exports after golem app deploy looks weird:
```
╔═
║ Created new component golem-qa:weather
║
║ Component name:    golem-qa:weather
║ Component ID:      9fe6128a-fde2-426d-bab8-88ffb8eeade1
║ Component type:    Durable
║ Component version: 0
║ Project ID:        6d1ba710-2967-4fc9-9c5b-f4d075ae9881
║ Component size:    2.82 MiB
║ Created at:        2025-09-02 14:50:33.247912574 UTC
║ Exports:
║   assistant-agent() agent constructor
║   assistant-agent.ask(name: string) -> return-value: string
║   weather-agent(username: string) agent constructor
║   weather-agent.getWeather(name: string, param2: record { data: string, value: s32 }) -> return-value: string
╚═
```

---

Creating a new worker using the cli fails with a scary error
```
❯ golem worker new weather-test
Selected profile: local
Creating new worker golem-qa:weather/weather-test

error: Worker Service - Error: 500 Internal Server Error, Invalid request: Invalid agent id: Invalid agent-id weather-test. Unexpected agent-id format - must be agent-type(...)

Exception: golem exited with 1
  [tty 16]:1:1-29: golem worker new wea
```

---

Creating worker does not accept "'" for strings
```
❯ golem worker new "golem-qa:weather/weather-agent('Maxim')"
Selected profile: local
Fuzzy matched pattern golem-qa:weather as golem-qa:weather
Creating new worker golem-qa:weather/weather-agent('Maxim')

error: Worker Service - Error: 500 Internal Server Error, Invalid request: Invalid agent id: Failed to parse parameter value 'Maxim': invalid token at 0..2
```

---

Showing of functions exported by worker is ugly due to the return types:
```
❯ golem worker invoke 'weather-agent("Maxim")'
Selected profile: local

error: the following required arguments were not provided:
  <FUNCTION_NAME>

Usage: golem worker invoke <WORKER_NAME> <FUNCTION_NAME> [ARGUMENTS]...

For more information, try '--help'.

Checking provided worker name: weather-agent("Maxim")
╔═
║ [ok] component: golem-qa:weather / worker: weather-agent("Maxim"), component was selected based on current dir
╚═

Available function names for component golem-qa:weather:
  - golem:agent/guest.{initialize}(agent-type: string, input: variant { tuple(list<variant { component-model(record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>), enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>),
     option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32), prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value:
     string }, u64>) }> }), unstructured-text(variant { url(string), inline(record { data: string, text-type: option<record { language-code: string }> }) }), unstructured-binary(variant { url(string), inline(record { data: list<u8>, binary-type: record { mime-type: string } }) }) }>), multimodal(list<tuple<string,
     variant { component-model(record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>), enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8),
     prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32), prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }), unstructured-text(variant { url(string), inline(record
     { data: string, text-type: option<record { language-code: string }> }) }), unstructured-binary(variant { url(string), inline(record { data: list<u8>, binary-type: record { mime-type: string } }) }) }>>) }) -> result<_, variant { invalid-input(string), invalid-method(string), invalid-type(string),
     invalid-agent-id(string), custom-error(record { value: record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>), enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>,
     option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32), prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }, typ: record { nodes: list<record
     { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>),
     prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> } }) }>
  - golem:agent/guest.{invoke}(method-name: string, input: variant { tuple(list<variant { component-model(record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>), enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>),
     result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32), prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }
     > }), unstructured-text(variant { url(string), inline(record { data: string, text-type: option<record { language-code: string }> }) }), unstructured-binary(variant { url(string), inline(record { data: list<u8>, binary-type: record { mime-type: string } }) }) }>), multimodal(list<tuple<string, variant
     { component-model(record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>), enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16),
     prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32), prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }), unstructured-text(variant { url(string), inline(record { data: string,
     text-type: option<record { language-code: string }> }) }), unstructured-binary(variant { url(string), inline(record { data: list<u8>, binary-type: record { mime-type: string } }) }) }>>) }) -> result<variant { tuple(list<variant { component-model(record { nodes: list<variant { record-value(list<s32>),
     variant-value(tuple<u32, option<s32>>), enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16),
     prim-s32(s32), prim-s64(s64), prim-float32(f32), prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }), unstructured-text(variant { url(string), inline(record { data: string, text-type: option<record { language-code: string }
     > }) }), unstructured-binary(variant { url(string), inline(record { data: list<u8>, binary-type: record { mime-type: string } }) }) }>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>), enum-value(u32),
     flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32), prim-float64(f64),
     prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }), unstructured-text(variant { url(string), inline(record { data: string, text-type: option<record { language-code: string }> }) }), unstructured-binary(variant { url(string), inline(record { data:
     list<u8>, binary-type: record { mime-type: string } }) }) }>>) }, variant { invalid-input(string), invalid-method(string), invalid-type(string), invalid-agent-id(string), custom-error(record { value: record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>),
     enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32),
     prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }, typ: record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>),
     enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type,
     prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> } }) }>
  - golem:agent/guest.{get-definition}() -> record { type-name: string, description: string, constructor: record { name: option<string>, description: string, prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>,
     owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type,
     prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record
     { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string,
     s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type,
     prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions:
     option<list<record { mime-type: string }>> }) }>>) } }, methods: list<record { name: string, description: string, prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type:
     variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type,
     prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }),
     unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string,
     option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type,
     prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }
     >>) }, output-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>),
     tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type,
     handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record
     { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>,
     option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record
     { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) } }>, dependencies: list<record { type-name: string, description: option<string>, constructor: record { name: option<string>, description: string,
     prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>),
     flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type,
     prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant
     { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32),
     result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }),
     unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) } }, methods: list<record { name: string, description: string, prompt-hint: option<string>, input-schema: variant
     { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>),
     list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum
     { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record
     { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>),
     prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions:
     option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) }, output-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>,
     type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type,
     prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }),
     unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string,
     option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type,
     prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }
     >>) } }> }> }
  - golem:agent/guest.{discover-agent-types}() -> list<record { type-name: string, description: string, constructor: record { name: option<string>, description: string, prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name:
     option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type,
     prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record
     { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string,
     s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type,
     prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions:
     option<list<record { mime-type: string }>> }) }>>) } }, methods: list<record { name: string, description: string, prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type:
     variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type,
     prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }),
     unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string,
     option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type,
     prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }
     >>) }, output-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>),
     tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type,
     handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record
     { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>,
     option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record
     { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) } }>, dependencies: list<record { type-name: string, description: option<string>, constructor: record { name: option<string>, description: string,
     prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>),
     flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type,
     prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant
     { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32),
     result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }),
     unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) } }, methods: list<record { name: string, description: string, prompt-hint: option<string>, input-schema: variant
     { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>),
     list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum
     { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record
     { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>),
     prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions:
     option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) }, output-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>,
     type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type,
     prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }),
     unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string,
     option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type,
     prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }
     >>) } }> }> }>
  - golem-qa:weather/assistant-agent.{initialize}() -> result<_, variant { invalid-input(string), invalid-method(string), invalid-type(string), invalid-agent-id(string), custom-error(record { value: record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>),
     enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32),
     prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }, typ: record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>),
     enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type,
     prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> } }) }>
  - golem-qa:weather/assistant-agent.{get-definition}() -> record { type-name: string, description: string, constructor: record { name: option<string>, description: string, prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name:
     option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type,
     prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record
     { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string,
     s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type,
     prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions:
     option<list<record { mime-type: string }>> }) }>>) } }, methods: list<record { name: string, description: string, prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type:
     variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type,
     prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }),
     unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string,
     option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type,
     prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }
     >>) }, output-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>),
     tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type,
     handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record
     { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>,
     option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record
     { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) } }>, dependencies: list<record { type-name: string, description: option<string>, constructor: record { name: option<string>, description: string,
     prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>),
     flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type,
     prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant
     { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32),
     result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }),
     unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) } }, methods: list<record { name: string, description: string, prompt-hint: option<string>, input-schema: variant
     { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>),
     list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum
     { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record
     { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>),
     prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions:
     option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) }, output-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>,
     type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type,
     prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }),
     unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string,
     option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type,
     prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }
     >>) } }> }> }
  - golem-qa:weather/assistant-agent.{ask}(name: string) -> result<string, variant { invalid-input(string), invalid-method(string), invalid-type(string), invalid-agent-id(string), custom-error(record { value: record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>),
     enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32),
     prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }, typ: record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>),
     enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type,
     prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> } }) }>
  - golem-qa:weather/weather-agent.{initialize}(username: string) -> result<_, variant { invalid-input(string), invalid-method(string), invalid-type(string), invalid-agent-id(string), custom-error(record { value: record { nodes: list<variant { record-value(list<s32>), variant-value(tuple<u32, option<s32>>),
     enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32), prim-s64(s64), prim-float32(f32),
     prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }, typ: record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>),
     enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type,
     prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> } }) }>
  - golem-qa:weather/weather-agent.{get-definition}() -> record { type-name: string, description: string, constructor: record { name: option<string>, description: string, prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name:
     option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type,
     prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record
     { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string,
     s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type,
     prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions:
     option<list<record { mime-type: string }>> }) }>>) } }, methods: list<record { name: string, description: string, prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type:
     variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type,
     prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }),
     unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string,
     option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type,
     prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }
     >>) }, output-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>),
     tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type,
     handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record
     { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>,
     option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record
     { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) } }>, dependencies: list<record { type-name: string, description: option<string>, constructor: record { name: option<string>, description: string,
     prompt-hint: option<string>, input-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>),
     flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type,
     prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant
     { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32),
     result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }),
     unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) } }, methods: list<record { name: string, description: string, prompt-hint: option<string>, input-schema: variant
     { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>),
     list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum
     { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record
     { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>),
     prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions:
     option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>) }, output-schema: variant { tuple(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>,
     type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type,
     prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }),
     unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }>>), multimodal(list<tuple<string, variant { component-model(record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>), variant-type(list<tuple<string,
     option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type, prim-s64-type, prim-f32-type,
     prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> }), unstructured-text(record { restrictions: option<list<record { language-code: string }>> }), unstructured-binary(record { restrictions: option<list<record { mime-type: string }>> }) }
     >>) } }> }> }
  - golem-qa:weather/weather-agent.{get-weather}(name: string, param2: record { data: string, value: s32 }) -> result<string, variant { invalid-input(string), invalid-method(string), invalid-type(string), invalid-agent-id(string), custom-error(record { value: record { nodes: list<variant { record-value(list<s32>),
     variant-value(tuple<u32, option<s32>>), enum-value(u32), flags-value(list<bool>), tuple-value(list<s32>), list-value(list<s32>), option-value(option<s32>), result-value(result<option<s32>, option<s32>>), prim-u8(u8), prim-u16(u16), prim-u32(u32), prim-u64(u64), prim-s8(s8), prim-s16(s16), prim-s32(s32),
     prim-s64(s64), prim-float32(f32), prim-float64(f64), prim-char(char), prim-bool(bool), prim-string(string), handle(tuple<record { value: string }, u64>) }> }, typ: record { nodes: list<record { name: option<string>, owner: option<string>, type: variant { record-type(list<tuple<string, s32>>),
     variant-type(list<tuple<string, option<s32>>>), enum-type(list<string>), flags-type(list<string>), tuple-type(list<s32>), list-type(s32), option-type(s32), result-type(tuple<option<s32>, option<s32>>), prim-u8-type, prim-u16-type, prim-u32-type, prim-u64-type, prim-s8-type, prim-s16-type, prim-s32-type,
     prim-s64-type, prim-f32-type, prim-f64-type, prim-char-type, prim-bool-type, prim-string-type, handle-type(tuple<u64, enum { owned, borrowed }>) } }> } }) }>



Exception: golem exited with 2
  [tty 21]:1:1-44: golem worker invoke 'weather-agent("Maxim")'
```

---

When invoking from the cli you still need WAVE. This feels very weird as the WebAssembly tooling is hidden everywhere else, but it's leaking when doing invocations.
```
❯ golem worker invoke 'weather-agent("Maxim")' 'golem-qa:weather/weather-agent.{get-weather}' '"Berlin"' '{ data: "A", value: 1 }'
Selected profile: local
Using generated idempotency key: b356eeca-9c2f-4b4a-8e84-7ad85d33bce7
Fuzzy matched pattern golem-qa:weather/weather-agent.{get-weather} as golem-qa:weather/weather-agent.{get-weather}
Invoking worker golem-qa:weather/weather-agent("Maxim")/golem-qa:weather/weather-agent.{get-weather}

Invocation results in WAVE format:
  - ok("Weather in Berlin is sunny.")
```

---

Having to pass constructor parameters for agents around everywhere feels very weird. IMO creating an agent and _always_ getting an id would be more intuitive

---

Template shows creating a singleton agent (assistant). Is this a reasonably pattern we want to recommend given that we have no way of running multiple concurrent invocations (?).

---

Using unsupported types leads to bad error messages

Code:
```typescript
class FooBar {

}

@agent()
class WeatherAgent extends BaseAgent {
    private readonly userName: string;

    constructor(username: FooBar) {
        super()
        this.userName = "";
    }
```

Output:
```
❯ golem app build
Selected profile: local
Resolving application wit directories
  Resolving component interface using base WASM for golem-qa:weather (/home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../../wasm/golem_agent.wasm, /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../../golem-temp/agents/golem_qa_weather/
       wit-generated)
  Extracting agent types defined in golem-qa:weather
Selecting components
  Found components: golem-qa:weather
  Selected components:
    golem-qa:weather: ts
Generating RPC artifacts
  Skipping creating generated base wit directory for golem-qa:weather, source WIT points to a WASM component
  Skipping creating generated wit directory for golem-qa:weather, no base WIT directory
Building components
  Building golem-qa:weather
    Executing external command 'npx --no golem-typegen components-ts/golem-qa-weather/tsconfig.json --files components-ts/golem-qa-weather/**/src/**/*.ts' in directory /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../..
ℹ Starting type metadata generation…
ℹ Processing 1 source files…
✔ Metadata tracked for: AssistantAgent, FooBar, WeatherAgent
ℹ Saving metadata…
✔ Metadata saved successfully in .metadata/generated-types.ts!
    Executing external command 'npx --no rollup -- -c' in directory /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather

.agent/main.ts → dist/main.js...
created dist/main.js in 337ms
    Injecting JS module /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/dist/main.js into QuickJS WASM /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../../wasm/golem_agent.wasm
    Generating agent wrapper for golem-qa:weather
      Extracting agent types from /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../../golem-temp/agents/golem_qa_weather.dynamic.wasm
      Generating agent WIT interface for golem-qa:weather
      Generating agent WIT interface implementation to /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../../golem-temp/agents/golem_qa_weather.wrapper.wasm
/tmp/.tmp5jwKbc/interface/golem/rpc/types/top.mbt:577:55-577:69 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmp5jwKbc/interface/golem/rpc/types/top.mbt:1995:67-1995:81 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmp5jwKbc/interface/golem/rpc/types/top.mbt:2789:43-2789:57 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmp5jwKbc/interface/golem/agent/common/top.mbt:48:50-48:61 [E0035] Warning: The word `constructor` is reserved for possible future use. Please consider using another name.
/tmp/.tmp5jwKbc/interface/golem/agent/common/top.mbt:52:49-52:60 [E0035] Warning: The word `constructor` is reserved for possible future use. Please consider using another name.
/tmp/.tmp5jwKbc/interface/golem/agent/guest/top.mbt:897:74-897:88 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmp5jwKbc/interface/golem/agent/guest/top.mbt:2011:92-2011:106 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmp5jwKbc/interface/golem/agent/guest/top.mbt:2270:92-2270:106 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmp5jwKbc/interface/golem/agent/guest/top.mbt:2552:74-2552:88 [E2000] Warning (Alert deprecated): Use `Int::unsafe_to_char` instead, and use `Int::to_char` for safe conversion
/tmp/.tmp5jwKbc/gen/interface/golem_qa/weather/assistantAgent/stub.mbt:6:28-6:77 [E0012] Warning: Unreachable code
/tmp/.tmp5jwKbc/gen/interface/golem_qa/weather/assistantAgent/stub.mbt:23:28-23:77 [E0012] Warning: Unreachable code
:0:0-0:0 [E0013] Warning: The type of this expression is (Array[(Unit, _/0)]) -> Map[Unit, _/0], which contains unresolved type variables. The type variable is default to Unit.
:0:0-0:0 [E0013] Warning: The type of this expression is (Array[(_/0, _/1)]) -> Map[_/0, _/1], which contains unresolved type variables. The type variable is default to Unit.
/tmp/.tmp5jwKbc/gen/interface/golem_qa/weather/weatherAgent/stub.mbt:1:19-1:27 [E0002] Warning: Unused variable 'username'
/tmp/.tmp5jwKbc/gen/interface/golem_qa/weather/weatherAgent/stub.mbt:5:42-5:49 [E0002] Warning: Unused variable 'builder'
/tmp/.tmp5jwKbc/gen/interface/golem_qa/weather/weatherAgent/stub.mbt:5:53-6:12 [E4014] `{}` means empty map instead of empty code block, use `()` for no-op.
```

---

We need to make clear that invoking an agent is an rpc boundary and involves copying data. People will try to share mutable data and it will fail

---

CustomData in example should use `type =` instead of `interface`

---

Types that reduce to simple, supported types are not handled way.
```typescript
import * as v from 'valibot';

const CustomDataSchema = v.object({
  data: v.string(),
  value: v.number(),
});

type CustomData = v.InferOutput<typeof CustomDataSchema>

```

Fails with:
```
❯ golem app build
Selected profile: local
Resolving application wit directories
  Resolving component interface using base WASM for golem-qa:weather (/home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../../wasm/golem_agent.wasm, /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../../golem-temp/agents/golem_qa_weather/
       wit-generated)
  Extracting agent types defined in golem-qa:weather
Selecting components
  Found components: golem-qa:weather
  Selected components:
    golem-qa:weather: ts
Generating RPC artifacts
  Skipping creating generated base wit directory for golem-qa:weather, source WIT points to a WASM component
  Skipping creating generated wit directory for golem-qa:weather, no base WIT directory
Building components
  Building golem-qa:weather
    Executing external command 'npx --no golem-typegen components-ts/golem-qa-weather/tsconfig.json --files components-ts/golem-qa-weather/**/src/**/*.ts' in directory /home/mschuwalow/Projects/golem-1.3.0-qa/components-ts/golem-qa-weather/../..
ℹ Starting type metadata generation…
ℹ Processing 1 source files…
/home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/@ts-morph/common/dist/ts-morph-common.js:443
            throw new InvalidOperationError(typeof errorMessage === "string" ? errorMessage : errorMessage(), node);
                  ^

InvalidOperationError: Expected to find the value declaration of symbol 'data'.
    at Object.throwIfNullOrUndefined (/home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/@ts-morph/common/dist/ts-morph-common.js:443:19)
    at Symbol.getValueDeclarationOrThrow (/home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/ts-morph/dist/ts-morph.js:17274:30)
    at file:///home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/@golemcloud/golem-ts-typegen/dist/index.js:195:54
    at Array.map (<anonymous>)
    at getFromTsMorph (file:///home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/@golemcloud/golem-ts-typegen/dist/index.js:194:45)
    at file:///home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/@golemcloud/golem-ts-typegen/dist/index.js:290:42
    at Array.map (<anonymous>)
    at updateMetadataFromSourceFiles (file:///home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/@golemcloud/golem-ts-typegen/dist/index.js:289:69)
    at Command.<anonymous> (file:///home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/@golemcloud/golem-ts-typegen/dist/bin/golem-typegen.js:20:5)
    at Command.listener [as _actionHandler] (/home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/commander/lib/command.js:569:17)

Node.js v22.16.0

error: Command failed with exit code: 1
```

---

No support for private functions that are not exposed in wit

code:
```typescript
@agent()
class WeatherAgent extends BaseAgent {
    private readonly userName: string;

    constructor(username: string) {
        super()
        this.userName = username;
    }

    @prompt("Get weather")
    @description("Weather forecast weather for you")
    async getWeather(name: string, param2: CustomData): Promise<string> {
        return Promise.resolve(
            `Weather in ${name} is sunny.`
        );
    }

    private foobar(obj: Object): Object {
      return obj
    }
}
```

output:
```
file:///home/mschuwalow/Projects/golem-1.3.0-qa/node_modules/@golemcloud/golem-ts-typegen/dist/index.js:238
    throw new Error("Unknown type: " + type.getText() + " with name: " + name);
```
---

.agent folder should probably not exist / be gitignored

---

We should have an http api example if we still care about it. In current rib instance is awkward with agents though

---

wasm rpc in the manifest does not work / fails with bad error messages

code:
```
dependencies:
  golem-qa:weather:
    - type: wasm-rpc
      target: golem-qa:second
```

output:
```
Generating RPC artifacts
  Skipping creating generated base wit directory for golem-qa:second, source WIT points to a WASM component
  Skipping creating generated base wit directory for golem-qa:weather, source WIT points to a WASM component

error: Failed to gather information for the stub generator: Failed to resolve wit dir: golem-temp/generated-base-wit/golem-qa_second: failed to parse package: golem-temp/generated-base-wit/golem-qa_second: failed to read directory "golem-temp/generated-base-wit/golem-qa_second": No such file
     or directory (os error 2)
```
