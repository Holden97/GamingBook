# 常用属性

## ContextMenu

<pre><code>[<a data-footnote-ref href="#user-content-fn-1">ContextMenu</a>("LoadFarm")]
public void LoadFarm()
{
    var curPlayerPoint = GameObject.FindGameObjectWithTag(Tags.PlayerRespawnPoint);
    SceneControllerManager.Instance.TryChangeScene(SceneEnum.Scene1_Farm.ToString(), curPlayerPoint.transform.position);
}
</code></pre>

[^1]: 
