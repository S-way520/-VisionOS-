# -VisionOS-
根据自己的开发经验（SwiftUI+RealityKit），简单聊聊如何开发VisionOS开发思维和开发工具
# 首先，假设一个问题：Vision Pro 与其他设备最大的区别是硬件还是开发环境？
为了回答这个问题，我将从开发思维和开发工具，简单聊聊VisionOS开发。
# 开发思维
1、直觉：声明式代码的开发很符合直接；基础AR模型的导入简单；基础AR模型动画的绑定很容易。
2、空间：依据RealityKit开发经验，空间画面的波动很小，且根据水平垂直锚点控制很轻松。
3、模型：以服务开发者为目的，让“高端操作”成为普通人能轻易完成的任务。
# 开发工具
1、RealityKit
这是我以往的开发：
```swift
import SwiftUI
import RealityKit
struct Reality: View {
    var body: some View {
        ARViewContainer().edgesIgnoringSafeArea(.all)
    }
}
struct ARViewContainer: UIViewRepresentable {
    func makeUIView(context: Context) -> ARView {
        let arView = ARView(frame: .zero, cameraMode: .ar, automaticallyConfigureSession: true)
        let anchorEntity = AnchorEntity(plane: .horizontal)
        let box = MeshResource.generateBox(size: 0.2/2, cornerRadius: 0.05/5)//形状
        let material = SimpleMaterial(color: .blue, isMetallic: true)//颜色
        let boxEntity = ModelEntity(mesh: box, materials: [material])//实体(正方体=颜色+形状)
        anchorEntity.addChild(boxEntity)
        arView.scene.anchors.append(anchorEntity)
        return arView
    }
    func updateUIView(_ uiView: ARView, context: Context) {
    }
}
```
这种声明很简单，我们修改内容时，只需要替换就好。
而现在，简单的导入AR模型，似乎更简单了：
```swift
import SwiftUI
import RealityKit
struct GlobeModule: View {
    var body: some View {
        Model3D(named: "Globe") { model in
            model
                .resizable()
                .scaledToFit()
        } placeholder: {
          	ProgressView()
        }
    }
}
```
2、SwiftUI
它一直都是很友好的开发方式：
```swift
import SwiftUI
struct ContentView: View {
    var body: some View {
        VStack {
            Image(systemName: "globe")//图片
                .imageScale(.large)
                .foregroundColor(.accentColor)
            Text("Hello, world!")//文字
        }
    }
}
```
3、Object Capture
这是一个拍照然后生成 usdz 模型的工具。普通人获取 usdz 模型的方式还有，模型制作网站https://www.vectary.com，模型下载网站https://sketchfab.com。
4、Reality Composer
这也是获得模型的一种方式，需要去Apple Store下载此app。Xcode也自带此程序。
5、Unity
这对游戏开发更友好，而简单的模型演示和行为绑定，可以使用Reality Composer完成。
6、Reality Composer Pro
这应该是 Reality composer 的增强版，而其中的能力在新版 Reality composer 中将会更新，要知道 swift Playground 与 XR 的配合远超 Xcode 与 XR 的配合，而 Reality composer 在 iPad 上的开发优势同样高于 Mac。
