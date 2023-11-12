To evaluate some code in Godot at runtime from the C++ 
side of the house, you can do this:

```c++
void eval (const char *expr) {
	auto script = memnew(GDScript);
	String tscript = String::utf8 (expr);
	script->set_source_code(tscript);
	script->reload();

        // Run the script by assigning it to a reference-counted object.
        Ref<RefCounted> ref_counted = memnew(RefCounted);
        ref_counted->set_script(script);
}
```

And then, make sure you call it with the `@tool` argument,
something like this:

```
@tool
extends RefCounted

func _init():
	code_you_want_to_call()
```