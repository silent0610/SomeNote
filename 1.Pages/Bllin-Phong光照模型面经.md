---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/23, 16:49:42
---

# Bllin-Phong光照模型面经

## 描述Phong光照模型 ？以及公式和代码？分别说一下Phong模型有哪些光照分量？说一下它们的作用是什么？

-  [[Phong Shading|Phong 模型]]
 Phong 模型是一种经验模型, 由三种光照组成
 - 环境光: 模拟间接光照, 即光照经过周围环境表面多次反射后形成的, 一般使用一个常量表示
 - 漫反射: 光照照射到表面随后被均匀的反射到各个方向上. 颜色取决于入射光线与表面法线之间的夹角和入射光线本身的强度
 - 镜面反射: 即高光, 若物体表面很光滑，在入射光线的反射方向上，具有较强的反射光线。与观察角度有关。大小取决于观察向量和反射向量的夹角大小, 以及入射角和表面夹角

$$
 L = k_{a}I_{a} + k_{d}I \max(0,\mathbf{n \cdot l})+ k_{s}I \max(0,\mathbf{v \cdot r})^{p} 
$$

其中, 反射向量

$$
\mathbf{r}=2(\mathbf{n} \cdot \mathbf{l}) \mathbf{n}-1
$$

k_s (**镜面反射系数**)：物体的材质属性，表示其产生高光的能力。通常是白色或灰色
k_d (**漫反射系数**)：物体的材质属性，表示它对光线的漫反射能力。这通常就是我们说的物体的“颜色”。

```glsl
void main()
{
    float ka = 0.1;
    vec3 ambient = ka * lightColor;

    float kd = 1.0;
    vec3 N = normalize(Normal);
    vec3 L = normalize(lightPos - FragPos);  
    float NdotL = max(dot(N, L), 0.0);
    vec3 diffuse = kd * NdotL * lightColor;

    float ks = 0.5;
    float p = 32;
    vec3 V = normalize(viewPos - FragPos);
    vec3 R = reflect(-L, N); 
    float spec = pow(max(dot(V, R), 0.0), p);
    vec3 specular = ks * spec * lightColor;  

    vec3 result = (ambient + diffuse + specular) * objectColor;
    FragColor = vec4(result, 1.0);
}
```

## 描述Bllin-Phong光照模型？以及公式和伪代码？

[[Blinn-Phong]]
模型的漫反射项、环境光项与Phong模型一样，区别在于高光项的计算，使用了半向量，避免计算反射向量。单光源的公式如下：

$$
L = k_{a}I_{a} + k_{d}I \max(0,\mathbf{n \cdot l})+ k_{s}I \max(0,\mathbf{n \cdot h})^{p}
$$

其中 

$$
\mathbf{h} = \frac{\mathbf{v + l}}{\Vert \mathbf{v + l} \Vert}
$$

```glsl
void main()
{
    float ka = 0.1;
    vec3 ambient = ka * lightColor;

    float kd = 1.0;
    vec3 N = normalize(Normal);
    vec3 L = normalize(lightPos - FragPos);  
    float NdotL = max(dot(N, L), 0.0);
    vec3 diffuse = kd * NdotL * lightColor;

    float ks = 0.5;
    float p = 32;
    vec3 V = normalize(viewPos - FragPos);
    vec3 H = normalize(L + V); 
    float spec = pow(max(dot(N, H), 0.0), p);
    vec3 specular = ks * spec * lightColor;  

    vec3 result = (ambient + diffuse + specular) * objectColor;
    FragColor = vec4(result, 1.0);
}
```

## Bllin-Phong和PBR的区别？

Bllin-Phong是一种**基于事实观察的经验模型**；而PBR是对光线、表面、相互作用进行量化的相对物理正确模型。

## 知道哪些光照模型（BRDF）？

- Phong，Bllin-Phong，
- [[Lambertian BRDF]]
- [[Cook-Torrance BRDF]]：常用的PBR模型
- Disney BRDF 是一个经验性、可调节、能统一多种材质表现的物理基础光照模型，广泛用于实时渲染中的通用材质系统。
- Ward BRDF 各项异性模型：用于模拟拉丝金属。

## Blinn-Phong和Phong的区别是什么，这种区别的作用是？

Blinn-Phong在计算高光项的时候，引入了半向量的概念，即观察向量和光线入射方向的一半。通过比较半向量和表面法线的对其程度（余弦），从而计算高光贡献值。可以避免计算反射向量，计算量更低。
- 半向量计算开销更低
- 效果上，**Phong的高光效果比Blinn-Phong更加真实**，尤其是模型背光处的高光效果。
