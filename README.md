# LineRenderer1
http://blog.csdn.net/zuoyamin/article/details/8997729

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class MouseControl : MonoBehaviour
{
	/// 直线渲染器
	[SerializeField]
	private LineRenderer lineRenderer;

	/// 是否第一次鼠标按下
	private bool firstMouseDown = false;

	/// 是否鼠标一直按下
	private bool mouseDown = false;

	void Update ()
	{
		if (Input.GetMouseButtonDown (0))
		{
			firstMouseDown = true;
			mouseDown = true;
		}
		if (Input.GetMouseButtonUp (0))
		{
			mouseDown = false;
		}
		onDrawLine ();
		firstMouseDown = false;
	}

	/// 保存的所有坐标
	private Vector3[] positions = new Vector3[10];

	/// 当前保存的坐标数量
	private int posCount = 0;
	/// 代表这一帧鼠标的位置 就 头的坐标
	private Vector3 head;

	/// 代表上一帧鼠标的位置
	private Vector3 last;
	/// 画线
	private void onDrawLine ()
	{
		if (firstMouseDown)
		{
			//先把计数器设为0
			posCount = 0;
			head = Camera.main.ScreenToWorldPoint (Input.mousePosition);
			last = head;
		}
		if (mouseDown)
		{
			head = Camera.main.ScreenToWorldPoint (Input.mousePosition);

			if (Vector3.Distance (head, last) > 0.01f)
			{
				savePosition (head);
				posCount++;
			}
			last = head;
		} else
		{
			positions = new Vector3[10];
		}
		changePositions (positions);
	}

	/// 保存坐标点
	private void savePosition (Vector3 pos)
	{
		pos.z = 0;

		if (posCount <= 9)
		{
			for (int i = posCount; i < 10; i++)
			{
				positions [i] = pos;
			}
		} else
		{
			for (int i = 0; i < 9; i++)
				positions [i] = positions [i + 1];

			positions [9] = pos;
		}
	}
	/// 修改直线渲染器的坐标
	private void changePositions (Vector3[] postions)
	{
		lineRenderer.SetPositions (postions);
	}
}



ray = Camera.main.ScreenPointToRay (Input.mousePosition);
		if (Physics.Raycast (ray,out hit)) {
			//控制手臂朝向碰撞点。
			m_Transform.LookAt (hit.point);
			//绘制测试线
			//Debug.DrawLine(m_Point.position,hit.point,Color.red);
			//设置LineRenderer的位置
			m_LineRenderer.SetPosition(0,m_Point.position);
			m_LineRenderer.SetPosition(1,hit.point);
