# ReGoap.FSMExample源码阅读小记

状态机部分

每一个minion(伐木者)身上都会挂载Idle和GoTo两个状态，当StateMachine在FixedUpdate中的Check方法收到state变为Pulsed的时候，会从Idle转变为GoTo，而当state变为非Active(!=GoToState.Active)时，会转为idle。
