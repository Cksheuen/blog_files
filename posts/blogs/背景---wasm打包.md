### 什么是`wasm`

`wasm`，全称`WebAssembly`（它的文件格式后缀是`.wasm`）。主要优势是作为二进制文件性能较原生的`js`等强大，实际应用中缺点是需要从`js`等获取数据导致的性能损失。

### RUST打包WASM

#### RUST环境配置

参见[《RUST Course》](https://course.rs/first-try/installation.html)，他讲的一定比我完善且好。而且他会向你推荐许多的`VSCode`插件，`rust`的相关生态在开发方面十分完善，体验非常好，一定能帮助`API`等的使用。

#### `wasm-bindgen`

具体详见[Hello, World! - The `wasm-bindgen` Guide](https://rustwasm.github.io/wasm-bindgen/examples/hello-world.html)。这里只提及一些重点部分。

##### 构建项目

高贵的~~前端O神玩家~~`rust`用户会使用自带的包管理工具`cargo new <project-name>`。

本项目需要用到的具体`Cargo.toml`配置如下：

```toml
[package]
name = "test"
version = "0.1.0"
edition = "2021"
build = "build.rs"

[build-dependencies]
aes-gcm = "0.9"
aes = "0.7"
generic-array = "0.14"
base64 = "0.13"

[lib]
crate-type = ["cdylib"]

[dependencies]
rand = "0.8.5"
wasm-bindgen = "0.2"
wasm-bindgen-test = "0.3.42"
getrandom = { version = "0.2.15", features = ["js"], optional = true }
js-sys = "0.3.69"
rayon = "1.5.1"
aes-gcm = "0.9"
base64 = "0.13"

[dependencies.web-sys]
version = "0.3.69"
features = [
    'Document',
    'Element',
    'HtmlElement',
    'Node',
    'Window',
    'CanvasRenderingContext2d',
    'Element',
    'KeyboardEvent',
    'HtmlCanvasElement',
    'WebGlBuffer',
    'WebGlVertexArrayObject',
    'WebGlRenderingContext',
    'WebGlProgram',
    'WebGlShader',
    'WebGlTexture',
    'ImageData',
    'WebGlRenderingContext',
    'Performance',
    'WebGlRenderingContext',
    'WebGlUniformLocation',
    'TextMetrics',
    'Performance',
]

[features]
default = ["getrandom/js"]
```

其中很长一列的`web-sys`下方的`feature`声明是它开启的功能（对，这个东西是选择性开启的），可以根据实际项目需求修改。

##### 具体实现

如何在`rust`中调用`js`。

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
extern "C" {
    #[wasm_bindgen(js_namespace = console, js_name = log)]
    fn log_u32(num: u32);
}

#[wasm_bindgen]
pub fn greet(name: &str) {
    alert(&format!("Hello, {}!", name));
}
```

其中`#[wasm_bindgen]`是一个关键词，代表需要从`js`中获取某些方法，或是将以下内容（可以是函数、类......）暴露给`js`（最终可以供你在前端调用）。其余部分就正常使用`rust`即可。

我们一般将主要需要的代码集中在`./src/lib.rs`中，需要本地使用的放在`./src/main.rs`中，其余模块根据需要新建。

#### `wasm-pack`

下载地址为[wasm-pack](https://rustwasm.github.io/wasm-pack/installer/)。

通过`wasm-bindgen`成功编写`rust`代码后，我们需要用`wasm-pack`来将它打包为`.wasm`文件与一系列相关的`js/ts`文件。具体操作为在`rust`项目的目录下运行`wasm-pack build`。然后就会生成一个`pkg`文件夹在目录下，其中就是可供前端调用的文件了。

### 在前端调用

和`rust`一样，开发时的痛苦就是为了使用时的 happy，只需要`import * as wasm from '..'`暴力调用即可。PS：`vite`等构建工具可能需要加载相应 plugin 帮助解析 `wasm`文件。

### 有关`glsl`等`static`的明文的加密

`wasm`打包后可以将代码变为二进制的`.wasm`文件，可以在一定程度上加密，但是仍然能够在`.wasm`文件中通过搜索获取到以下内容（可读但不完全可读的`glsl`代码内容）。

```
  (data (i32.const 1048684) "\01\00\00\00\08\00\00\00encryption failure!src\5clib.rs\00\00\00\87\00\10\00\0a\00\00\00)\00\00\00\0a\00\00\00attribute vec4 a_position;\0d\0avoid main() { gl_Position = a_position; }\0d\0aprecision mediump float;\0d\0a\0d\0auniform vec3 iResolution;\0d\0auniform float iTime;\0d\0a\0d\0auniform vec3 ro;\0d\0auniform vec3 ta;\0d\0auniform mat3 camMat;\0d\0a//------------------------------------------------------------------------\0d\0a// SDF sculpting\0d\0a//\0d\0a// Thsi is the SDF function F that defines the shapes, in this case a sphere\0d\0a// of radius 1. More info: https://iquilezles.org/articles/distfunctions/\0d\0a//---------------------------
```

若需要更加完善的加密，我们只需要在`rust`代码中增加一个`main.rs`（实际运行代码）即可，并在其中对`glsl`代码进行加密，将`wasm-bindgen`中引用的代码修改为加密后代码再在之后解密即可。

虽然但是，加密的`key`显然也存在代码中最终也会被打包至`.wasm`文件，这又该这么解决呢？

欸！很简单，我们直接将需求的KEY也设置为不可读的样子不就行了。如：

```rust
const KEY: &[u8; 32] = b"\x00\x0c\x00\x00\x00\x44\x00\x00\x00\x12\x00\x00\x00\x34\x00\x00\x00\x56\x00\x00\x00\x78\x00\x00\x00\x9a\x00\x00\x00\xbc\x00\x00";
const NONCE: &[u8; 12] = b"\x00\x0a\x00\x0b\x00\x0c\x00\x0d\x00\x0e\x00\x0f";

```

这样我们就完美地将`glsl`代码加密了，而付出的主要代价仅仅是在编译时多运行一段代码`cargo run`。

当然，为了便携性，我们果断选择为`wasm-pack build`新增一个前置。

首先，我们在`rust`项目的根目录新建一段类似下方所示的`build.rs`代码。

```rust
use std::env;
use std::fs;

use aes_gcm::aead::{generic_array::GenericArray, Aead, NewAead};
use aes_gcm::Aes256Gcm; // Or another AES variant
use base64;

const KEY: &[u8; 32] = b"\x00\x0c\x00\x00\x00\x44\x00\x00\x00\x12\x00\x00\x00\x34\x00\x00\x00\x56\x00\x00\x00\x78\x00\x00\x00\x9a\x00\x00\x00\xbc\x00\x00";
const NONCE: &[u8; 12] = b"\x00\x0a\x00\x0b\x00\x0c\x00\x0d\x00\x0e\x00\x0f";

fn encrypt_glsl_code(glsl_code: &str) -> String {
    let cipher = Aes256Gcm::new(GenericArray::from_slice(KEY));
    let ciphertext = cipher
        .encrypt(GenericArray::from_slice(NONCE), glsl_code.as_bytes())
        .expect("encryption failure!");
    base64::encode(&ciphertext)
}

fn main() {
    let current_dir = env::current_dir().expect("Unable to get current directory");
    let output_dir = current_dir.join("glsl");

    let vertex_shader_path = output_dir.join("shader.vert");
    let fragment_shader_path = output_dir.join("shader.frag");

    let vertex_shader_source =
        fs::read_to_string(vertex_shader_path).expect("Unable to read vertex shader file");
    let fragment_shader_source =
        fs::read_to_string(fragment_shader_path).expect("Unable to read fragment shader file");

    let encrypted_vertex_shader = encrypt_glsl_code(&vertex_shader_source);
    let encrypted_fragment_shader = encrypt_glsl_code(&fragment_shader_source);

    let secret_vertex_shader_path = output_dir.join("secret.vert");
    let secret_fragment_shader_path = output_dir.join("secret.frag");

    fs::write(secret_vertex_shader_path, encrypted_vertex_shader)
        .expect("Unable to write secret vertex shader file");
    fs::write(secret_fragment_shader_path, encrypted_fragment_shader)
        .expect("Unable to write secret fragment shader file");
}

```

之后我们需要在`Cargo.toml`中新增`build`配置，新增在构建项目（此处即指`wasm-pack build`）前运行的默认脚本。这样我们就能在原有的操作下获得完整的加密体验。

```toml
[package]
# ...existing configuration...
build = "build.rs"

[build-dependencies]
aes-gcm = "0.9"
aes = "0.7"
generic-array = "0.14"
base64 = "0.13"
```

