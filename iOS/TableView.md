테이블 뷰에서 각 셀마다 내용물의 크기가 다른경우, 내용물의 크기, constraints에 따라 셀의 크기를 지정해줘야 할 때가 있다.

해결방법으로는 TableView의 rowHeight속성에 AutometicDimension을 통해 테이블의 row가 유동적이라는 것을 선언해 줘야한다.

>예시
```swift
 func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        return tableView.rowHeight
    }
 ```
   혹은
```swift
   override func viewDidLoad(){
   	super.viewDidLoad();
    	myTableView.rowHeight = UITableView.automaticDimension
   }
```
위와같은식으로 지정하게 되면 원래 지정해놓았던 row의 height값을 무시하고 각 row안의 내용에 따라 높이가 유동적으로 결정된다.


### 애플 공식 문서

- #### rowHeight
![](https://images.velog.io/images/tnddls2ek/post/c0f7603c-8358-40f1-8fa4-6ec31622ec4e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-06-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.01.32.png)

>
설명: 
이 속성을 사용하여 테이블보기의 셀에 대한 사용자 지정 높이를 지정합니다. 이 속성의 기본값은 automaticDimension이며, 테이블보기에서 셀 내용에 따라 적절한 높이를 선택합니다.

- #### automaticDimension
![](https://images.velog.io/images/tnddls2ek/post/245a948e-31f2-48f2-a030-544df8c9802f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-06-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.20.51.png)

>
설명:
테이블 뷰가 주어진 차원에 대한 기본값을 선택하도록하려는 경우 테이블 뷰의 델리게이트 메서드에서이 값을 반환합니다. 예를 들어 tableView (_ : heightForHeaderInSection :) 또는 tableView (_ : heightForFooterInSection :)에서이 상수를 반환하는 경우 테이블 뷰는 tableView (_ : titleForHeaderInSection :) 또는 tableView (_ : titleForFooterInSection에서 반환 된 값에 맞는 높이를 사용합니다. :), 제목이 nil이 아닌 경우.

- #### estimatedRowHeight
![](https://images.velog.io/images/tnddls2ek/post/c450810d-0d3b-45b1-aacf-eea96d8e846d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-06-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.24.27.png)
>
설명:
행 높이의 음수가 아닌 추정치를 제공하면 테이블보기로드 성능을 향상시킬 수 있습니다. 테이블에 가변 높이 행이 포함 된 경우 테이블이로드 될 때 모든 높이를 계산하는 데 비용이 많이들 수 있습니다. 추정을 사용하면로드 시간에서 스크롤링 시간까지 기하학 계산 비용의 일부를 연기 할 수 있습니다.
기본값은 automaticDimension이며, 이는 테이블보기가 사용자 대신 사용할 예상 높이를 선택 함을 의미합니다. 값을 0으로 설정하면 예상 높이가 비활성화되어 테이블 뷰가 각 셀의 실제 높이를 요청하게됩니다. 테이블에서 자체 크기 조정 셀을 사용하는 경우이 속성의 값은 0이 아니어야합니다.
높이 추정을 사용할 때 테이블보기는 스크롤보기에서 상속 된 contentOffset 및 contentSize 속성을 적극적으로 관리합니다. 이러한 속성을 직접 읽거나 수정하지 마십시오.
**요약**
대충 요약하자면 가변적인 높이를 가진 셀들이 테이블뷰에 존재한다면, 높이값의 추정치를 설정해주는 역할. 높이값의 추정치를 estimatedRowHeight에 설정해주게 되면 tableView를 load할때에 성능을 향상시킬 수 있다고 함
**또한, 값을 0으로 설정할 경우 높이를 예상하는부분이 비활성화되어 셀마다의 실제 높이를 요청하게 됨**

- estimatedRowHeight 설정 방법
```swift
//직접 대입
tableView.estimatedRowHeight = 500

혹은

//델리게이트 메서드 사용
func tableView(_ tableView: UITableView, estimatedHeightForRowAt indexPath: IndexPath) -> CGFloat {
	return 500
}
```
로 **직접 대입**하거나 **델리게이트 메서드**를 이용해 설정할 수 있따!


### 가끔 생기는 문제점
동적으로 셀을 만들고, 접었다 펼때 애니메이션 효과때문에 화면이 깜빡이거나 통통 튀는 현상이 발생한다.

### 해결할 수 있는 4가지 방법


- heightForRowAt에서 정확한 높이값을 지정 -> 확실히 해결가능
- tableView.reloadData() -> 어느정도 해결가능
- tableView.estimatedSectionHeaderHeight = 0
tableView.estimatedSectionHeaderHeight = 0 -> 어느정도 경우 해결가능
- UIView.setAnimationsEnabled(true or false) -> 확실히 해결가능

[참고 블로그](https://blog.naver.com/jdub7138/220963701224)
