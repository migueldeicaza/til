When I first started with RealityKit, there was a lack of guidance on how to
design applications using their [entity
system](https://developer.apple.com/documentation/realitykit/system) and how to
design [components](https://developer.apple.com/documentation/realitykit/system)
for it.

I tried looking up assorted other resources, but while there is a wealth of
guidance on how to optimize these things, I did not know how to apply them to my
application.

I ended up with a design where I keep using object-oriented patterns for my
high-level objects, but I attach some behavior using components.

I am using systems in this case to perform updates that run on each timer tick,
for example, I want my entities to track a particular position, so I created a
"LookAtComponent", which merely contains a target:

```
struct LookAtComponent: Component {
    var target: SIMD3<Float>
}
```

And a system that goes along with it:

```
public class LookAtSystem : System {
    public required init(scene: Scene) {
    }

    static let query = EntityQuery(where: .has(LookAtComponent.self))
    static let target = SIMD3<Float> (0, 0, 0)
    
    public func update(context: SceneUpdateContext) {
        angle += Float (context.deltaTime)
        context.scene.performQuery(Self.query).forEach { entity in
            guard let lookAtComponent = entity.components [LookAtComponent.self] else {
                return
            }
            entity.look(at: lookAtComponent.target, from: entity.position, relativeTo: nil)
        }
    }
}
```

So at least, I am starting to get a feeling of how to attach behaviors to 
entities in this way.   

My code looks roughly like this:

```
// I create these objects that hold on to a top-level entity
// and I add behaviors to it, in this case the LookOut Compoent
class MyObject {
    var entity: Entity

    init () {
	self.entity = someEntityModelThatICreatedElsewhere ()
	let lookAtComponent = LookAtComponent (target: SIMD3<Float> (0, 0, 0))
	self.entity.components.set (self.lookAtComponent)
    }

    // This method is part of my OO design
    func explode () {
	// run some exploding animation
    }
}
```


Maybe one day I will move all of my custom logic
to these things, but for now, I am keeping my blend of objects and entities
going.

