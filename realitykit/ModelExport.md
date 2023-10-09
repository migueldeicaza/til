# Simple Model Exporter

If you have RealityKit model and you want to see what is in it,
you can use the following function to export it to the Wavefront format:

```swift
public enum ExportError: Error {
    case ioError
}

public func exportMesh (model: ModelComponent, to: String) throws {
    FileManager.default.createFile(atPath: to, contents: nil)
    guard let handle = FileHandle(forWritingAtPath: to) else {
        throw ExportError.ioError
    }
    try bufferedWriter(on: handle) { writer in
        for x in model.mesh.contents.models {
            for part in x.parts {
                if let vertices = part.buffers [.positions]?.get (SIMD3<Float>.self) {
                    for vertix in vertices {
                        try writer ("v \(vertix.x) \(vertix.y) \(vertix.z)\n")
                    }
                }
                
                if let triangles = part.buffers [.triangleIndices]?.get (UInt16.self) {
                    let e = triangles.elements
                    for idx in stride(from: 0, to: triangles.count, by: 3) {
                        try writer ("f \(e [idx]+1) \(e[idx+1]+1) \(e[idx+2]+1)\n")
                    }
                } else if let triangles = part.buffers [.triangleIndices]?.get (UInt32.self) {
                    let e = triangles.elements
                    for idx in stride(from: 0, to: triangles.count, by: 3) {
                        try writer ("f \(e [idx]+1) \(e[idx+1]+1) \(e[idx+2]+1)\n")
                    }
                }
                
            }
        }
    }
}
```

This uses my helper [BufferedIO routine](../swift/BufferedIO.md)