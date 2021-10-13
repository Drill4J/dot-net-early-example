# Drill4J .NET early implementation capabilities

This repository serves as demonstration of a general approach of Drill4J's .NET support.

> This is WIP and _does not fully represent neither the current nor final state of the .NET implementation_. Consider this a sneak peek.
>
> We may expand this repo as new features are developed.

## Injected DLLs example

- __Drill4Net.Target.Common.dll__ is the ["original" DLL](./injection-example/Drill4Net.Target.Common.dll)

- __Drill4Net.Target.Common(injected).dll__ is [DLL injected with Drill4J instructions](./injection-example/Drill4Net.Target.Common(injected).dll)

You can inspect both DLL contents with tool such as [__dnSpy__](https://github.com/dnSpy/dnSpy) or [__ILSpy__](https://github.com/icsharpcode/ILSpy)

## dnSpy usage example

1. Open dnSpy

2. In the top menu click "File -> Open"

3. In the dialog window locate and select __Drill4Net.Target.Common(injected).dll__, click "Open"

4. Scroll down in the left menu and locate `Drill4Net.Target.Common (0.0.0.0)` entry

    ![`Drill4Net.Target.Common (0.0.0.0)`](./media/dnspy-1.png)

5. Expand it, then expand Drill4Net.Target.Common.dll

    ![`Drill4Net.Target.Common (0.0.0.0)` - expanded view](./media/dnspy-2.png)

6. You can explore entries under `Drill4Net.Target.Common` by clicking on each one. Search for `"ProfilerProxy::Register"`  with CTRL+F to see the kind of changes that Drill4J introduced.

    ![`Drill4Net.Target.Common (0.0.0.0)` - example search](./media/dnspy-3.png)
    _example of `"ProfilerProxy::Register"` search in `GenStr`_

7. __Extended explanation:__ The `Injection_*guid*/ProfilerProxy` is the special inject type which enables communication with Drill4J backend

    In fully decompiled view such  will look like that

    ```
    "ProfilerProxy.Register("d58c6d72-3b5b-4a4f-97b1-8d2cb84c025a^Drill4Net.Target.Common.dll^...^Return_7/6");"
    ```

    And in in CIL code it will be look like this
    ```
    IL_0016: ldstr     "d58c6d72-3b5b-4a4f-97b1-8d2cb84c025a^Drill4Net.Target.Common.dll^...^Return_7/6"
    IL_001B: call      void [Drill4Net.Target.Common.dll]Injection_d1dc3f4d5e574718a56e8599559cd0ed.ProfilerProxy::Register(string)
    ```

    _Note_: additionally, there are also some assemblies loaded by late binding
