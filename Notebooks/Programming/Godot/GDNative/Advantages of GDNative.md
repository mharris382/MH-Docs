#godot #scripting #gdnative 

seealso: [[Advantages of C++]]

### Advantages of GDNative[¶](https://docs.godotengine.org/en/stable/tutorials/scripting/gdnative/what_is_gdnative.html#advantages-of-gdnative "Permalink to this headline")


Unlike modules, GDNative doesn't require compiling the engine's source code, making it easier to distribute your work. It gives you access to most of the API available to GDScript C#, allowing you to code game logic with full control regarding performance. It's ideal if you need high-performance code you'd like to distribute as an add-on in the [asset library](https://docs.godotengine.org/en/stable/community/asset_library/what_is_assetlib.html#doc-what-is-assetlib).

Also:

-   GDNative is not limited to C and C++. Thanks to [third-party bindings](https://docs.godotengine.org/en/stable/tutorials/scripting/gdnative/what_is_gdnative.html#doc-what-is-gdnative-third-party-bindings), you can use it with many other languages.
    
-   You can use the same compiled GDNative library in the editor and exported project. With C++ modules, you have to recompile all the export templates you plan to use if you require its functionality at run-time.
    
-   GDNative only requires you to compile your library, not the whole engine. That's unlike C++ modules, which are statically compiled into the engine. Every time you change a module, you need to recompile the engine. Even with incremental builds, this process is slower than using GDNative.