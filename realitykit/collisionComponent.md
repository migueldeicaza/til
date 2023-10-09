When you add a collision component to an entity, it will add the "CollisionComponent" only to those elements that can get one:

> For non-model entities, the method has no effect. Nevertheless, the method is 
> defined for all entities so that you can call it on any entity, and have the 
> calculation propagate recursively to all that entity’s descendants. 

The challenge is that if you later attempt to lookup the entity that triggered
the collision, you will not get the toplevel container, but the child that
caused it.    In my case, I was updating the position of it, but this was
updating merely the child, not the container, so everything got out of sync.
 
