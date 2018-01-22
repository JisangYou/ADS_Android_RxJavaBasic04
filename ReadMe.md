# ADS04 Android

## 수업 내용

- RxBinding을 학습

## Code Review

### MainActivity

```Java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);


        // 0. 로그인 체크하기
        Observable<TextViewTextChangeEvent> idEmitter = RxTextView.textChangeEvents(findViewById(R.id.editId));
        Observable<TextViewTextChangeEvent> pwEmitter = RxTextView.textChangeEvents(findViewById(R.id.editPw));

        // 조건 id 가 5자이상이고, pw가 8자이상면 btnSignin 의 enable 을 true 로 아니면 false
        Observable.combineLatest(
                idEmitter,
                pwEmitter,
                (TextViewTextChangeEvent id, TextViewTextChangeEvent pw) -> {
                    boolean idCheck = id.text().length() >= 5;
                    boolean pwCheck = pw.text().length() >= 8;

                    if (idCheck && pwCheck)
                        return true;
                    else
                        return false;
                }
        ).subscribe(
                (Boolean flag) -> {
                    findViewById(R.id.btnSignin).setEnabled(flag);
                }
        );


        // 1. editText에 입력되는 값을 체크해서 실시간으로 Log를 뿌려준다.
        RxTextView.textChangeEvents(findViewById(R.id.editText))
                .subscribe(ch -> Log.i("RxBinding", "word:" + ch.text()));

        // 2. 버튼을 클릭하면 editText에 Random 숫자를 입력
        RxView.clicks(findViewById(R.id.button))
                // button 에는 Button 타입의 오브젝트가 리턴되는데, 이를 문자타입으로 변경
                .map(button -> new Random().nextInt() + "")
                // subscribe 에서는 map 에서 변형된 타입을 받게 된다.
                .subscribe(number -> {
                    ((EditText) findViewById(R.id.editText)).setText(number);
                });
    }
}
```


## 보충설명

### RxBinding이란?

> 버튼, 스크롤, 여러 이벤트를 옵저버블로 처리해주는 라이브러리. 버튼 클릭, 스크롤 상태에 대한 처리, 기타 뷰 이벤트 처리에 모두 사용할 수 있음.



### 출처

- 출처 : https://academy.realm.io/kr/posts/rxjava-in-action/

## TODO

- binding에 대한 공부

## Retrospect

- 추후에 프로젝트에 사용하면 유용할 것 같다.

## Output
- 생략
