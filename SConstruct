

import SCons
env = Environment()
env.Tool('textfile')
def _add_scanner(builder):
    def new_scanner(node, env, path, argument):
        deps = argument(node, env, path)
        for i, dep in enumerate([str(f) for f in deps]):
            if dep == "myheader.h":
                del deps[i]
        env.Requires(node, "myheader.h")
        return deps

    # The 'builder.builder' here is because we need to reach inside
    # the CompositeBuilder that wraps the object builders that come
    # back from createObjBuilders.
    builder.builder.source_scanner = SCons.Scanner.Scanner(
        function=new_scanner,
        path_function=builder.source_scanner.path_function,
        argument=builder.source_scanner,
    )

for object_builder in SCons.Tool.createObjBuilders(env):
    _add_scanner(object_builder)

env.Textfile('myheader.h', ['#pragma once', '//comment', 'const char* test = "test";'])
env.Program('out', 'main.c')