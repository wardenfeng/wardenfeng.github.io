# Fagal

Fagal是渲染代码AGAL处理工具库，从feng3d项目中孕育而出，灵感来自于Cg语言实现过程吸收部分EasyAGAL，所以Fagal比EasyAGAL更Easy，目标是像Cg语言那样容易编写flash3D的渲染程序。

在away3d中的AGAL渲染代码几乎到了无法看懂的程度，并且寄存器的分配也是写死了的，这样非常容易弄错也非常地不灵活。为了解决这个问题，我研究出了Fagal，它非常容易看懂、分配寄存器再也不用我们来操心了、甚至连数据的更新与提交stage3d过程也不用操心了，使用起来极其方便。

[Fagal首个实例BaseShaderTest](blogs/2014/10/27/1.md)的渲染函数类代码

```
package configs
{

	/**
	 * 缓冲编号配置文件
	 * @author warden_feng 2015-7-23
	 */
	public class Context3DBufferIDConfig
	{
		public static const bufferIdConfigs:Array = [ //
			["index", "索引数据"], //
			["program", "渲染程序"], //
			["position_va_3", "顶点坐标数据"], //
			["color_va_3", "顶点颜色数据"], //
			["projection_vc_matrix", "顶点程序投影矩阵静态数据"], //
			["color_v", "颜色变量寄存器"], //
			["_op", "顶点输出寄存器"], //
			["_oc", "片段输出寄存器"], //
			//------------------------
			//		地形渲染相关
			//------------------------
			["terrainTextures_fs_array", "地形纹理数组"], //
			["uv_va_2", "uv数据"], //
			["uv_v", "uv变量数据"], //
			//------------------------
			//		地形渲染相关
			//------------------------
			["commonsData_vc_vector", "公用数据片段常量数据"], //
			];
	}
}
```

```
package fagal
{
	import flash.display3D.Context3DProgramType;

	import me.feng3d.fagal.methods.FagalMethod;
	import me.feng3d.fagalRE.FagalRE;

	/**
	 * 基础顶点渲染
	 * @author warden_feng 2014-10-24
	 */
	public class V_baseShader extends FagalMethod
	{
		public function V_baseShader()
		{
			_shaderType = Context3DProgramType.VERTEX;
		}

		override public function runFunc():void
		{
			var _:* = FagalRE.instance.space;

			_.comment("应用投影矩阵", _.projection_vc_matrix, "使世界坐标", _.position_va_3, "转换为投影坐标 并输出到顶点寄存器", _._op);
			_.m44(_._op, _.position_va_3, _.projection_vc_matrix);

			_.comment("传递顶点颜色数据", _.color_va_3, "到变量寄存器", _.color_v);
			_.mov(_.color_v, _.color_va_3);
		}
	}
}
```

```
package fagal
{
	import flash.display3D.Context3DProgramType;

	import me.feng3d.fagal.methods.FagalMethod;
	import me.feng3d.fagalRE.FagalRE;

	/**
	 * 基础片段渲染
	 * @author warden_feng 2014-10-24
	 */
	public class F_baseShader extends FagalMethod
	{
		public function F_baseShader()
		{
			_shaderType = Context3DProgramType.FRAGMENT;
		}

		override public function runFunc():void
		{
			var _:* = FagalRE.instance.space;

			_.comment("传递顶点颜色数据", _._oc, "到片段寄存器", _.color_v);
			_.mov(_._oc, _.color_v);
		}
	}
}
```
V_baseShader与F_baseShader的渲染log
自动分配寄存器

```
------------Compiling Register info------------------
projection_vc_matrix:vc0[顶点程序投影矩阵静态数据]
_oc:oc[片段输出寄存器]
color_v:v0[颜色变量寄存器]
_op:op[顶点输出寄存器]
position_va_3:va0[顶点坐标数据]
color_va_3:va1[顶点颜色数据]
```

可查看调试用的渲染log

```
Compiling FAGAL Code:
--------------------
//应用投影矩阵 projection_vc_matrix 使世界坐标 position_va_3 转换为投影坐标 并输出到顶点寄存器 _op
m44 _op, position_va_3, projection_vc_matrix
//传递顶点颜色数据 color_va_3 到变量寄存器 color_v
mov color_v, color_va_3
--------------------
//传递顶点颜色数据 _oc 到片段寄存器 color_v
mov _oc, color_v
```

用于上传至Context3D的AGAL

```
Compiling AGAL Code:
--------------------
//应用投影矩阵 vc0 使世界坐标 va0 转换为投影坐标 并输出到顶点寄存器 op
m44 op, va0, vc0
//传递顶点颜色数据 va1 到变量寄存器 v0
mov v0, va1
--------------------
//传递顶点颜色数据 oc 到片段寄存器 v0
mov oc, v0
------------Compiling info end------------------
```

## Fagal项目进度

2014.3.14-2014.10.22：Fagal隐身于feng3d，已经实现按名称自动分配寄存器，自动更新渲染时所用数据（顶点坐标、uv、投影矩阵等等一切渲染时所需数据）等功能。

2014.10.22-2014.10.27：Fagal独立于feng3d，利用EasyAGAL部分代码包装AGAL汇编语言，模仿Cg语言实现AGALMethod类，制作BaseShaderTest实例。

2014.10.27-2014-12-26：优化Fagal，使用元标签反射进行自动申请寄存器。Fagal源码地址：http://pan.baidu.com/s/1kTKaKK3  Fagal库编译参数需加上 -keep-as3-metadata+=Register,AGALMethod

2014-12-26-2015-07-02：优化Fagal，使用Context3DBufferTypeID属性代替 元标签方式自动申请寄存器。 实现Context3DBufferDebug用于调试Fagal。

2015-07-02-now：优化Fagal，使用Context3DBufferIDConfig类进行缓冲编号配置，自动释放不再使用的临时寄存器，优化获取缓冲过程，优化Fagal相关log。

## 最新代码：
[http://git.oschina.net/wardenfeng/feng3d](http://git.oschina.net/wardenfeng/feng3d) （必要文件夹 FagalTest、Fagal、fengCommon）

参考：

1. [EasyAGAL](https://github.com/Barliesque/EasyAGAL)
2. Cg语言