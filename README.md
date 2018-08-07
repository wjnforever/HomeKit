# HomeKit
Apple iOS HomeKit

HomeKit 密码学安全机制的设计： 使用Ed25519做公钥签名/验证，使用SRP(3072 bit)在iOS设备和HomeKit配件之间交换密码并做认证，使用ChaCha20-Poly1305做对称加密(普适性；ARM v8之后加入了AES指令，所以在这些平台上的设备，AES方式反而比chacha20-Poly1305方式更快，性能更好，或者设备自带硬件AES模块)，使用HKDF-SHA512做密钥拓展。每个session的开始用Station-to-Station协议做密钥协商和认证，随后使用Curve25519做密钥协商，生成共享key。
(BLE Authentication靠脸，Numeric Comparison/Passkey Entry面对面进行；ECDH密钥交换没问题；AES-128貌似不妥，AES-256没问题；AES-CMAC；为何不AES-GCM)

HomeKit provides a home automation infrastructure that utilizes iCloud and iOS security to protect and synchronize private data without exposing it to Apple.
… HomeKit identity and security are based on Ed25519 public-private key pairs. An Ed25519 key pair is generated on the iOS device for each user for HomeKit, which becomes his or her HomeKit identity. It is used to authenticate communication between iOS devices, and between iOS devices and accessories.
… 
Communication with HomeKit accessories HomeKit accessories generate their own Ed25519 key pair for use in communicating with iOS devices. To establish a relationship between an iOS device and a HomeKit accessory, keys are exchanged using Secure Remote Password (3072-bit) protocol, utilizing an 8-digit code provided by the accessory's manufacturer and entered on the iOS device by the user, and then encrypted using ChaCha20-Poly1305 AEAD with HKDF-SHA-512-derived keys. The accessory's MFi certification is also verified during setup. When the iOS device and the HomeKit accessory communicate during use, each authenticates the other utilizing the keys exchanged in the above process. Each session is established using the Station-to-Station protocol and is encrypted with HKDF-SHA-512 derived keys based on per-session Curve25519 keys. This applies to both IP-based and Bluetooth Low Energy accessories.

"Apple's HomeKit protocol supports both IP and BLE devices. While there appear to be a few open source implementations of IP stacks around, I couldn't find any BLE stacks. So, here's one for you to play with."
原作者参考HomeKit protocols for IP，基于Nordic nrf51/52平台的一个实现.
