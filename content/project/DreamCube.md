---
title: "드림큐브"
date: 2022-10-03T22:50:08+09:00
draft: false
tags: ["project"]
---
### 개요
AR 오브젝트와 실제 공간이 동기화되어 움직이는 신개념 AR 콘텐츠

### 기본정보
| | |
|-------------|----------------------------------------------------------|
| 개발인원 / 기간 |2명(개발1, 디자인1) / 20.11~21.04(6개월)|
|개발 환경| Unity3D, Vuforia SDK, Android|
|기능| Unity3D 시스템 / Object Detection/tracking / Shader, Particle 시스템|
|역할 / 기여도| 메인 개발자 / 50%|
|개발 목적| 회사 신규 IP 개발, 교육 사업 콘텐츠 개발,AR 응용 기술력 확보|

### 아이디어
디자이너와 함께 생각되는 아이디어를 직접 그려보며 브레인스토밍을 통해 아이디어를 구체화 하였습니다.
기존의 모니터에서만 존재하던 AR 콘텐츠를 넘어서 실제 접촉가능한 물체와 AR오브젝트가 동시에 움직인다면 
더 신기한 사용자 경험을 제공할 수 있지 않을까 생각하게 된 아이디어였습니다. 

### 주요 결과물
- 시연영상 : https://www.youtube.com/watch?v=fD2mmF4WhxY
- APK : https://drive.google.com/u/1/uc?id=1mZ0Hef_GeA39Ux0wbl5FTkl7hMTPxSwF&export=download


### 주요 코드
#### SceneTouch.cs
화학 콘텐츠에서 주변의 화학물질을 고를때 사용자 인터페이스 구현

```C#
public class SceneTouch : MonoBehaviour
{
    public UIManager _UIM;
    public Transform Top;
    public Transform Bottom;
    public Transform Left;
    public Transform Right;
    public Text Debuging;
    Vector2 preclickpos = Vector2.zero;
    float speed = 0.5f;
    float zoom_speed = 0.05f;
    float now_Scale = 0.15f;
    private float std_height;
    private float std_width;
    //초기 해상도 대응
    void Start()
    {
        std_height = (-41f * (Screen.height / 1920f));
        std_width = (-40f * (Screen.width / 1080f));
    }

    void Update()
    {
        Vector3 finalPos = Vector3.zero;
//유니티 에디터 테스트용 구현
#if UNITY_EDITOR
        if (Input.GetMouseButton(0))
        {
            Vector2 clickpos = Input.mousePosition;
            if (preclickpos == Vector2.zero)
                preclickpos = clickpos;
            Vector2 deltapos = (preclickpos - clickpos) * speed;
//시점 변경
            Vector3 TopPos = Camera.main.WorldToViewportPoint(Top.position);
            Vector3 BottomPos = Camera.main.WorldToViewportPoint(Bottom.position);
            Vector3 LeftPos = Camera.main.WorldToViewportPoint(Left.position);
            Vector3 RightPos = Camera.main.WorldToViewportPoint(Right.position);
            Debug.Log(RightPos.x + " / " + std_width);
            if (LeftPos.x < 0f)
            {
                Debug.Log("Left Over");
                deltapos.x = 1;
            }
            if (RightPos.x >= -41f)
            {
                Debug.Log("Right Over");
                deltapos.x = -1;
            }
            if (BottomPos.y < 0f)
            {
                Debug.Log("Bottom Over");
                deltapos.y = 1;
            }
            if (TopPos.y >= std_height)
            {
                Debug.Log("Top Over");
                deltapos.y = -1;
            }
            //Debug.Log(deltapos);
            preclickpos = clickpos;
            finalPos = _UIM.PicTotal_Trans.position - new Vector3(deltapos.x, deltapos.y, _UIM.PicTotal_Trans.position.z);
            _UIM.PicTotal_Trans.position = finalPos;
        }
        else
        {
            preclickpos = Vector2.zero;
        }
//줌 기능 구현
        if (Input.GetAxis("Mouse ScrollWheel") < 0)
        {  // Zoom Out
            _UIM.Gesture_icon.SetActive(false);
            if (_UIM.PicTotal_Trans.localScale.x >= 0.15f)
            {
                now_Scale -= zoom_speed;
                _UIM.PicTotal_Trans.localScale = new Vector3(now_Scale, now_Scale, now_Scale);
            }
        }
        if (Input.GetAxis("Mouse ScrollWheel") > 0)
        { // Zoom In
            _UIM.Gesture_icon.SetActive(false);
            now_Scale += zoom_speed;
            _UIM.PicTotal_Trans.localScale = new Vector3(now_Scale, now_Scale, now_Scale);
        }
#endif

//모바일 환경 기능 구현
#if UNITY_ANDROID
        Transform trans = _UIM.PicTotal_Trans.transform;
        float coe = 0.001f;
        //손가락 1개 이동 기능 구현
        if (Input.touchCount == 1)
        {
            _UIM.Gesture_icon.SetActive(false);
            Touch touchZero = Input.GetTouch(0);
            Vector2 pos = touchZero.deltaPosition;
            Debuging.text = (trans.localPosition.x).ToString() + " / " + (trans.localPosition.y).ToString() + " / " + (trans.localPosition.z).ToString();
            if (trans.localPosition.x >= 800f)
            {
                pos.x = -3;
            }
            else if (trans.localPosition.x <= -800f)
            {
                pos.x = 3;
            }
            if (trans.localPosition.y >= 500f)
            {
                pos.y = -3;
            }
            else if (trans.localPosition.y <= -500f)
            {
                pos.y = 3;
            }

            _UIM.PicTotal_Trans.position = _UIM.PicTotal_Trans.position + new Vector3(pos.x, pos.y, _UIM.PicTotal_Trans.position.z);
        }
        //줌 확대 축소 구현
        if (Input.touchCount == 2) //손가락 2개가 눌렸을 때
        {
            Touch touchZero = Input.GetTouch(0); //첫번째 손가락 터치를 저장
            Touch touchOne = Input.GetTouch(1); //두번째 손가락 터치를 저장

            //터치에 대한 이전 위치값을 각각 저장함
            //처음 터치한 위치(touchZero.position)에서 이전 프레임에서의 터치 위치와 이번 프로임에서 터치 위치의 차이를 뺌
            Vector2 touchZeroPrevPos = touchZero.position - touchZero.deltaPosition; //deltaPosition는 이동방향 추적할 때 사용
            Vector2 touchOnePrevPos = touchOne.position - touchOne.deltaPosition;

            // 각 프레임에서 터치 사이의 벡터 거리 구함
            float prevTouchDeltaMag = (touchZeroPrevPos - touchOnePrevPos).magnitude; //magnitude는 두 점간의 거리 비교(벡터)
            float touchDeltaMag = (touchZero.position - touchOne.position).magnitude;

            // 거리 차이 구함(거리가 이전보다 크면(마이너스가 나오면)손가락을 벌린 상태_줌인 상태)
            float deltaMagnitudeDiff = prevTouchDeltaMag - touchDeltaMag;
            if (trans.localScale.x >= 0.15f)
            {
                trans.localScale = new Vector3(trans.localScale.x - (deltaMagnitudeDiff * coe), trans.localScale.y - (deltaMagnitudeDiff * coe), trans.localScale.z - (deltaMagnitudeDiff * coe));
            }
            else
            {
                trans.localScale = new Vector3(0.15f, 0.15f, 0.15f);
            }




        }
#endif
    }
}
```
</details>

#### LunarChecker.cs
AR의 오브젝트들이 월식 상황으로 되어있는지 확인하는 함수 

```c#
public class LunarChecker : MonoBehaviour
{
    private RaycastHit hit;
    public bool isHitPart;
    public bool isHitTotal;
    public Color RayColor;
    public LayerMask PARTLM;
    public LayerMask TOTALLM;
    public Transform SunSpere;
    public Transform  MoonSpere;
    public Transform  EarthSpere;

    public string eclipseLv = "none";

    float MaxDistance = 100f;

    // Update is called once per frame
    void Update()
    {
        Debug.DrawRay(transform.position, -transform.forward * MaxDistance, RayColor, 0.3f);
        //지구 시점에서 달의 부분 월식 범위 이상 가려져 있는지 확인
        if (Physics.Raycast(transform.position, -transform.forward, out hit, MaxDistance, PARTLM))
        {
            if (hit.collider.gameObject.tag == "Moon")
            {
                isHitPart = true;
                //moonCol.radius = 0.3f;
            }
            else
            {
                isHitPart = false;
            }
        }
        //지구 시점에서 달의 개기 월식 범위 이상 가려져 있는지 확인
        if (Physics.Raycast(transform.position, -transform.forward, out hit, MaxDistance, TOTALLM))
        {
            if (hit.collider.gameObject.tag == "Moon")
            {
                isHitTotal = true;
            }
            else
            {
                isHitTotal = false;
                //moonCol.radius = 1.3f;
            }
        }
        StatueUpdater();
        float offsetX = SunSpere.position.x - EarthSpere.position.x;
        float offsetZ = SunSpere.position.z - EarthSpere.position.z;
        float offsetY = EarthSpere.position.y - MoonSpere.position.y;
        transform.LookAt(new Vector3(EarthSpere.position.x+offsetX,EarthSpere.position.y+offsetY,EarthSpere.position.z + offsetZ));
    }

    public void StatueUpdater()
    {
        //현재 월식 상황 업데이트
        if (isHitPart)
        {
            if (!isHitTotal)
            {
                eclipseLv = "Partial";
            }
            else
            {
                eclipseLv = "Total";
            }
        }
        else
        {
            eclipseLv = "none";
        }
        isHitPart = false;
        isHitTotal = false;
    }

    public int pic_checker()
    {

        int result = 0;
        switch (eclipseLv)
        {
            case "none":
                result = 0;
                break;
            case "Partial":
                result = 2;
                break;
            case "Total":
                result = 1;
                break;
        }
        return result;
    }
}
```