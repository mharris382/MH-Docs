#godot #gdnative #cpp #scripting 
# [gdnative](https://docs.godotengine.org/en/stable/tutorials/scripting/gdnative/index.html)
- [What is gdnative?](https://docs.godotengine.org/en/stable/tutorials/scripting/gdnative/what_is_gdnative.html)
- [GDNative C Example](https://docs.godotengine.org/en/stable/tutorials/scripting/gdnative/gdnative_c_example.html)
- [GDNative C++ example](https://docs.godotengine.org/en/stable/tutorials/scripting/gdnative/gdnative_cpp_example.html)



# What is gdnative?
**GDNative** is a Godot-specific technology that lets the engine interact with native [shared libraries](https://en.wikipedia.org/wiki/Library_(computing)#Shared_libraries) at run-time. You can use it to run native code without compiling it with the engine.

> GDNative is _not_ a scripting language and has no relation to [GDScript](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html#doc-gdscript).

**You can use both GDNative and [C++ modules](https://docs.godotengine.org/en/stable/development/cpp/custom_modules_in_cpp.html#doc-custom-modules-in-c) to run C or C++ code in a Godot project.**

They also both allow you to integrate third-party libraries into Godot. The one you should choose depends on your needs.

---

Unlike modules, GDNative doesn't require compiling the engine's source code, making it easier to distribute your work.


| GDNative                   | C++                   |
| -------------------------- | --------------------- |
| ![[Advantages of GDNative]] | ![[Advantages of C++]] | 
