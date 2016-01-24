---
title: cocos2dx 如何生成适合 wp8 的预编译 shader
tags: cocos2dx
date: 2014-08-04
---

# wp8 上如何预编译shader

此文首次发布在 http://www.cocoachina.com/bbs/read.php?tid-218745.html

# 写在最前面, 
1. 当然是吐槽啦, 为什么官方不直接提供预编译版本的 `angle\src\winrtcompiler\winrtcompiler.exe`
2. 当然, 官方还是提供了 `cocos2d-x\build\winrt\Cocos2dShaderCompiler` 这个工程让我们编译自己的shader, 但是这个工具的易用性确实有待提高.

# 编译的两种方式

## 修改 Cocos2dShaderCompiler
首先你得琢磨一下怎么使用这个工具. 

打开工程, 找到 ShaderCompiler.cpp, 修改之

```c++
bool ShaderCompiler::Compile(Windows::UI::Xaml::Controls::Te [attachment=75109] xtBlock^ resultText)
{
    m_resultText = resultText;
    resultText->Text = "Compiling shaders...";

    if (!InitializeAngle(ANGLE_D3D_FEATURE_LEVEL::ANGLE_D3D_FEATURE_LEVEL_9_3))
    {
        resultText->Text = "Unable to initialize Angle";
        return false;
    }

    Director::getInstance()->setAnimationInterval(1.0 / 60.0);
    CCShaderCache::getInstance()->loadDefaultShaders();

    // 找到文件夹路劲, 把 shader 丢进去
    const std::vector<std::string>& pathes = FileUtils::getInstance()->getSearchPaths();
    for (auto path : pathes)
    {
        log(">>> [ %s ]", path.c_str());
    }
    
    CCGLProgram *pGLProgram = new CCGLProgram();
    pGLProgram->initWithFilenames(
        FileUtils::getInstance()->fullPathForFilename( "first.vert" ),
        FileUtils::getInstance()->fullPathForFilename( "first.frag" ));
    pGLProgram->link();
    pGLProgram->updateUniforms();
    CCShaderCache::getInstance()->addGLProgram(pGLProgram, "first");

    CCPrecompiledShaders::getInstance()->savePrecompiledShaders();
    resultText->Text = "Complete";
    return true;
}
```

"可执行文件" 只能在模拟器中运行, fvck.

## 使用 winrtcompiler.exe 生成

根据 http://www.cocos2d-x.org/wiki/How_to_update_wp8_shader 的提示, 下载 ANGLE 包

```shell
git clone https://github.com/MSOpenTech/angle
```

1. 打开 `angle\src\winrtcompiler\winrtcompiler_2013.sln` 工程 (提示, 不能使用 express 版本打开)
2. 修改 main.cpp, 添加两段代码

	2.1 在以下代码
	 
	```c++
	[Platform::MTAThread]
	int __cdecl main(int argc, char* argv[])
	```

	前添加以下代码

	```
	//
	// 需要跟随引擎更新这段代码..
	//
	enum
	{
	    VERTEX_ATTRIB_POSITION,
	    VERTEX_ATTRIB_COLOR,
	    VERTEX_ATTRIB_TEX_COORD,
	    VERTEX_ATTRIB_NORMAL,
	    VERTEX_ATTRIB_BLEND_WEIGHT,
	    VERTEX_ATTRIB_BLEND_INDEX,
	
	    VERTEX_ATTRIB_MAX,
	
	    // backward compatibility
	    VERTEX_ATTRIB_TEX_COORDS = VERTEX_ATTRIB_TEX_COORD,
	};
	void bindPredefinedVertexAttribs(GLuint program)
	{
	    static const struct {
	        const char *attributeName;
	        int location;
	    } attribute_locations[] =
	    {
	        { "a_position", VERTEX_ATTRIB_POSITION },
	        { "a_color", VERTEX_ATTRIB_COLOR },
	        { "a_texCoord", VERTEX_ATTRIB_TEX_COORD },
	        { "a_normal", VERTEX_ATTRIB_NORMAL },
	    };
	
	    const int size = sizeof(attribute_locations) / sizeof(attribute_locations[0]);
	
	    for (int i = 0; i<size; i++) {
	        glBindAttribLocation(program, attribute_locations.location, attribute_locations.attributeName);
	    }
	}
	```

	2.2 在以下代码

	```c++
	glLinkProgram(program);
	```

	之前添加代码:

	```
	//
	// copy from cocos2d-x v3
	//
	bindPredefinedVertexAttribs(program);
	```

编译出来的是 win32 可执行文件! 附上可执行文件下载地址  http://pan.baidu.com/s/1ntuRQu1

`angle\src\winrtcompiler\winrtcompiler.bat` 这个文件里有如何创建 winrt 和 wp8 shader 的示例.

我把代码贴出来:

```
bin\winrtcompiler.exe -o=shader_winrt.h -p=winrt -a=gProgram -v=shader.vert -f=shader.frag
bin\winrtcompiler.exe -o=shader_wp8.h -p=wp8 -a=gProgram -v=shader.vert -f=shader.frag
```

然后需要把预编译的 shader 添加到引擎内部, 代码? 稍等 ~

# 总结
两种方式各有利弊
方式1可以方便地自动添加 shader 数据到 precompiledheader.h 中去, 但app比较难用
方式2比较容易使用, 修改添加shader的方式来支持动态加载 shader, 不知道会不会有其他坑

# 参考资料
1. [url]http://www.cocos2d-x.org/wiki/How_to_update_wp8_shader[/url]
2. [url]http://www.ispinel.com/2014/07/03/12393[/url]


