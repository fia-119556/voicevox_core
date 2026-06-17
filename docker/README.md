# 16KB対応 AAR作成

## Docker起動
```bash
$docker compose up -d
$docker exec -it voicevox_android_build bash
```

## so作成
ターゲット追加
```
$rustup target add aarch64-linux-android
```
ビルド
```bash
$cargo build \
  --release \
  --target aarch64-linux-android \
  -p voicevox_core_java_api
```

### 出力場所
target/aarch64-linux-android/release

### 16KB確認
aarを展開、コマンドを実行し、`LOAD`の`Align`が`0x4000 (16384)` であることを確認
```bash
$readelf -l libvoicevox_core_java_api.so
```

## aar作成
soを配置
```bash
$mkdir crates/voicevox_core_java_api/lib/src/main/resources/jniLibs/arm64-v8a

$cp \
  target/aarch64-linux-android/release/libvoicevox_core_java_api.so \
  crates/voicevox_core_java_api/lib/src/main/resources/jniLibs/arm64-v8a

$cp \
  /opt/android-ndk/toolchains/llvm/prebuilt/linux-x86_64/sysroot/usr/lib/aarch64-linux-android/libc++_shared.so \
  crates/voicevox_core_java_api/lib/src/main/resources/jniLibs/arm64-v8a

```
ビルド
```bash
$cd crates/voicevox_core_java_api
$OS=android ./gradlew build
```

### 出力場所
crates/voicevox_core_java_api/lib/build/outputs/aar
