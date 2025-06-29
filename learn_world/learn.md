```XML
<mujoco model="inverted_pendulum">
    <compilter angle="radian/degree" autolimits="true" > 
```

    compilter编译器节点：角度制 autolimit是受力限制
    ```XML
 <option timestep = "0.002" gravity="0 0 -9.81" integrator="implicitfast">
 ```
 option节点 timestep代表仿真走一步的时间，也就是运行一次之后仿真计算出timestep时长后的世界，单位秒。timestep是一定要规定的，不然仿真不知道如何计算
    integrator 是积分器的方法：最好用implicitfast
    solver 是求解器的方法：默认是牛顿
```XML
    <visual>
        <global realtime="1" />
        ```
        这个是仿真速度比例，是与真实世界的比例
        ```XML
        <quality shadowsize="16384" numslices="28" offsamples="4" />
        ```
        shadowsize是阴影细节
        ```XML
        <headlight diffuse="1 1 1" specular="0.5 0.5 0.5" active="1" />
        ```
        这个active是开关头灯 diffuse是漫反射光的rgb specular是镜面反射的光
        ```XML
        <rgba fog="1 0 0 1" haze="1 1 1 1" />
    </visual>
```
    <asset>
        <mesh name="tetrahedron" vertex="0 0 0 1 0 0 0 1 0 0 0 1" />这个mesh既可以自己导入obj/stl也可以设 vertex vertex是坐标的数组，这些坐标连接成一个几何体
        <mesh file="card.obj" scale="1 1 1" />
        <texture type="2d" file="./king_of_clubs.png" />
        <material name="king_of_clubs" texture="king_of_clubs" />
        <hfield name="agent_eval_gym" file="agent_eval_gym.png" size="10 10 1 1" />
        <texture type="skybox" file="../asset/desert.png"
            gridsize="3 4" gridlayout=".U..LFRB.D.." />

        <!-- <texture type="skybox" builtin="flat" rgb1="1 1 .1" rgb2=".9 .9 .9" width="128" height="128"/> -->
        <texture name="plane" type="2d" builtin="checker" rgb1=".1 .1 .1" rgb2=".9 .9 .9"
            width="512" height="512" mark="cross" markrgb=".8 .8 .8" />
        <material name="plane" reflectance="0.3" texture="plane" texrepeat="1 1" texuniform="true" />
        <material name="box" rgba="0 0.5 0 1" emission="0" />
    </asset>

    <default>
        <geom solref=".5e-4" solimp="0.9 0.99 1e-4" fluidcoef="0.5 0.25 0.5 2.0 1.0" />
        <default class="card">
            <geom type="mesh" mesh="card" mass="1.84e-4" fluidshape="ellipsoid" contype="0"
                conaffinity="0" />
        </default>
        <default class="collision">
            <geom type="box" mass="0" size="0.047 0.032 .00035" group="3" friction=".1" />
        </default>
    </default>

    <worldbody>
        <geom name="floor" pos="0 0 0" size="0 0 .25" type="plane" material="plane"
            condim="3" />
        <light directional="true" ambient=".3 .3 .3" pos="30 30 30" dir="0 -2 -1"
            diffuse=".5 .5 .5" specular=".5 .5 .5" />

        <body pos="0 0 1" euler="-45 45 0">
            <freejoint />
            <geom class="card" material="king_of_clubs" />
            <geom class="collision" />
        </body>

        <geom type="hfield" hfield="agent_eval_gym" pos="0 11 0" />

        <body pos="-1 0 .5">
            <freejoint />
            <geom type="sphere" size="0.1" rgba=".5 0 0 1" />
        </body>
        <body pos="-0.5 0 .5">
            <freejoint />
            <geom type="box" size="0.1 0.1 0.1" material="box" />
        </body>
        <body pos="0 0 .5">
            <freejoint />
            <geom type="capsule" size="0.1 0.1" rgba="0 0 .5 1" />
        </body>
        <body pos=".5 0 .5">
            <freejoint />
            <geom type="cylinder" size="0.1 0.1" rgba=".5 .5 0 1" />
        </body>
        <body pos="1 0 .5">
            <freejoint />
            <geom type="ellipsoid" size="0.2 0.2 0.1" rgba="0 .5 .5 1" />
        </body>
        <body pos="1.5 0 .5">
            <freejoint />
            <geom type="ellipsoid" size="0.2 0.1 0.1" rgba=".5 0 .5 1" />
        </body>

        <body pos="2.0 0 .5">
            <freejoint />
            <geom type="mesh" mesh="tetrahedron" rgba=".5 .5 .5 1" />
        </body>
    </worldbody>
</mujoco>
python3 -m mujoco.viewer运行
