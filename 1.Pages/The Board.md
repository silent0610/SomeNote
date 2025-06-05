---
Type:
  - Page
---

## 1. A Tower Defense Game

## 2. PathFinding

使用广度优先搜索

## 3. Changing Tiles

### 3.2 Content Factory

[什么是内容工厂](什么是内容工厂.md)
只需要一个负责销毁内容 Factory

### 3.3 Touching a Tile

- 简单来说, 这节实现了一个函数, 根据鼠标点击位置cast ray(这部分还未实现), 并返回对应的tile  

```cpp
    public GameTile GetTile(Ray ray)
    {
        if (Physics.Raycast(ray, out RaycastHit hit))
        {
            // 由于tile 长宽都是 1 ,这样可以计算出水平和垂直的坐标
            int x = (int)(hit.point.x + size.x * 0.5f);
            int y = (int)(hit.point.z + size.y * 0.5f);
            if (x >= 0 && x < size.x && y >= 0 && y < size.y)
            {
                return tiles[x + y * size.x];
            }
        }
        return null;
    }
```

### 3.4 Changing Content

通过鼠标点击 创建 目标content

### 3.5 Keeping a Valid Board

保持 Board 有效
在 删除最后一个dest时添加回去. 

## 4 Walls

墙壁, 防御塔只能放在墙上

### 4.1 Content

### 4.5 Showing the Grid

[URP不支持standard\_shader](URP不支持standard_shader.md)

> [!caution] URP 不支持 standard shader
> 所以下面这段代码无效

```cs
	Material m = ground.GetComponent<MeshRenderer>().material;
	if (showGrid) {
		m.mainTexture = gridTexture;
	}
	else {
		m.mainTexture = null;
	}
```

需要替换为

```cs
	Material m = ground.GetComponent<MeshRenderer>().material;
	if (showGrid)
	{
		m.SetTexture("_BaseMap", gridTexture);
		m.SetTextureScale("_BaseMap", size);
	}
	else
	{
		m.SetTexture("_BaseMap", null);
	}
```
