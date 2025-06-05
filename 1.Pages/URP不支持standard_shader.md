---
Type:
  - Page
---

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
