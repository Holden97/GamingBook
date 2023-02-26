# 物理场景模拟

模拟示例

<pre><code>    private Scene _simulationScene;
<strong>    private PhysicsScene2D _physicsScene;
</strong>
<strong>    _simulationScene = SceneManager.CreateScene("Simulation", new CreateSceneParameters(LocalPhysicsMode.Physics2D));
</strong><strong>    _physicsScene = _simulationScene.GetPhysicsScene2D();
</strong>
    public void SimulateBall(BallPlayer ghost, Vector3 pos, GameObject line)
    {
        //clear;
        ClearProjectionLine();

        var ghostObj = Instantiate(ghost, pos, Quaternion.identity);
        //特别注意，如果是通过Rigidbody的力来模拟，那么一定需要先将模拟物体拖入对应物理场景，再施加力，反之无效。
        //直接修改位置则不受此规则影响。
        SceneManager.MoveGameObjectToScene(ghostObj.gameObject, _simulationScene);
        var ghostDir = (Camera.main.ScreenToWorldPoint(startPos) - Camera.main.ScreenToWorldPoint(endPos)).normalized;
        Debug.Log($"ghostDir:{ghostDir}");
        ghostObj.GhostPop(new BallDirEventArg(ghostDir));
        //ghost.CurSpeed = ghostDir * 10f;

        var hitCount = 0;
        Debug.Log("-----------------------本次模拟线");
        for (int i = 0; i &#x3C; 40; i++)
        {
            _physicsScene.Simulate(Time.fixedDeltaTime);
            //if (ghostObj.GhostSimulate(_physicsScene))
            //{
            //    hitCount++;
            //}

            Quaternion dirRotation = Quaternion.FromToRotation(Vector3.up, ghostObj.ballDir);

            var emptyObj = NextObject;

            if (emptyObj)
            {
                emptyObj.SetActive(true);
                emptyObj.transform.SetPositionAndRotation(ghostObj.transform.position, dirRotation);
            }
            else
            {
                var go = Instantiate(line, ghostObj.transform.position, Quaternion.identity, this.transform);
                go.transform.localScale = Vector3.one * 0.5f;
                go.transform.rotation = dirRotation;
                projects.Add(go);
            }


            if (hitCount >= 2)
            {
                break;
            }
        }

        Destroy(ghostObj.gameObject);
    }
</code></pre>
