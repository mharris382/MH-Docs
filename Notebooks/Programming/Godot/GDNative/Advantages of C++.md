#godot #scripting #cpp

seealso: [[Advantages of GDNative]]

### Advantages of C++ modules[¶](https://docs.godotengine.org/en/stable/tutorials/scripting/gdnative/what_is_gdnative.html#advantages-of-c-modules "Permalink to this headline")

We recommend [C++ modules](https://docs.godotengine.org/en/stable/development/cpp/custom_modules_in_cpp.html#doc-custom-modules-in-c) in cases where GDNative isn't enough:

-   C++ modules provide deeper integration into the engine. GDNative's access is limited to what the scripting API exposes.
    
-   You can use C++ modules to provide additional features in a project without carrying native library files around. This extends to exported projects.
    
-   C++ modules are supported on all platforms. In contrast, GDNative has only limited support on HTML5 (cannot be used together with multi-threading), and is not supported on Universal Windows Platform (UWP).
    
-   C++ modules can be faster than GDNative, especially when the code requires a lot of communication through the scripting API.