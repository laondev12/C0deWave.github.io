---
layout: default
title:  TableView 활용하기
parent: IOS
nav_order: 19
---

# TableView 활용하기

테이블 뷰란 Todo리스트와 같이 무언가 일을 하거나 전화번호부와 같은 리스트를 제공하는 기능을 담당합니다.

<img width="480" alt="스크린샷 2020-08-03 오후 2 20 56" src="https://user-images.githubusercontent.com/16849874/89148376-992d9980-d594-11ea-98ed-44c33a75297d.png">

위와 같은 기능을 담당합니다.

<img width="288" alt="스크린샷 2020-07-31 오후 3 14 31" src="https://user-images.githubusercontent.com/16849874/89148465-e4e04300-d594-11ea-9d84-f15e65888fc1.png">

먼저 스토리 보드를 만들때 Table Controller을 만든 다음에 Is Initial ViewController을 눌러서 시작 뷰로 지정해 줍니다.

<img width="580" alt="스크린샷 2020-07-31 오후 3 15 41" src="https://user-images.githubusercontent.com/16849874/89148528-13f6b480-d595-11ea-9ac7-1f64336bb61a.png">

위와 같이 뷰를 만듭니다.

<img width="813" alt="스크린샷 2020-07-31 오후 3 21 14" src="https://user-images.githubusercontent.com/16849874/89148557-283ab180-d595-11ea-92a4-2d02bd2c1abf.png">

그후 control - 드래그를 해서 show를 눌러서 연결을 해줍니다.

<img width="1038" alt="스크린샷 2020-07-31 오후 3 22 39" src="https://user-images.githubusercontent.com/16849874/89148620-54eec900-d595-11ea-8999-2f470b2b17d9.png">

연결이 완료가 되면 이러한 이미지가 됩니다.

<img width="1533" alt="스크린샷 2020-07-31 오후 3 26 18" src="https://user-images.githubusercontent.com/16849874/89148656-73ed5b00-d595-11ea-834a-4a636bed2daa.png">

위에 바 버튼 아이템으로 버튼을 만들어서 연결하면 됩니다.

그후 스토리보드를 다음과 같이 꾸며줍니다.

<img width="802" alt="스크린샷 2020-07-31 오후 3 31 02" src="https://user-images.githubusercontent.com/16849874/89148753-ae56f800-d595-11ea-8f8b-9d6a6881b5ab.png">

그후 각각의 컨트롤러를 만들어 줍니다.

UIViewController로 DetailViewController과 AddViewController두개를 만들어 줍니다.

같은 방식으로 TableViewController도 만들어 줍니다.

<img width="722" alt="스크린샷 2020-07-31 오후 3 31 22" src="https://user-images.githubusercontent.com/16849874/89148845-e9592b80-d595-11ea-8616-1019604988ed.png">

<img width="708" alt="스크린샷 2020-07-31 오후 3 32 44" src="https://user-images.githubusercontent.com/16849874/89148880-07bf2700-d596-11ea-9692-142957adcf37.png">

그후 위와같이 각각의 View에 맞는 컨트롤러를 지정해 줍니다.

이제부터는 코드를 살펴 보도록 하겠습니다.

메인 화면의 코드 입니다.

```swift
import UIKit

var items = ["책 구매","철수와 약속","스터디 준비하기"]
var itemsImageFile = ["cart.png","clock.png","pencil.png"]

class TableViewController: UITableViewController {
    
    @IBOutlet var tvListView: UITableView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        //바 버튼을 이용해서 문제 해결하기
        self.navigationItem.leftBarButtonItem = self.editButtonItem
    }

    // MARK: - Table view data source

    //테이블의 섹션의 개수 입니다.
    override func numberOfSections(in tableView: UITableView) -> Int {
        // #warning Incomplete implementation, return the number of sections
        return 1
    }

    //섹션당 열의 개수 입니다.
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // #warning Incomplete implementation, return the number of rows
        return items.count
    }

    //선언한 변수를 적용시킵니다.
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "myCell", for: indexPath)

        //셀의 텍스트 레이블에 앞에서 선언한 items를 대입합니다.
        cell.textLabel?.text = items[(indexPath as NSIndexPath).row]
        //셀의 이미지뷰 앞에서 선언한 이미지를 대입합니다.
        cell.imageView?.image = UIImage(named: itemsImageFile[(indexPath as NSIndexPath).row])
        
        return cell
    }
    
    //삭제를 한글로 만드는 함수
    override func tableView(_ tableView: UITableView, titleForDeleteConfirmationButtonForRowAt indexPath: IndexPath) -> String? {
        return "삭제"
    }

    //추가된 내용 새로고침 함수
    override func viewWillAppear(_ animated: Bool) {
        tvListView.reloadData()
    }

    //목록 삭제 코딩하기
    // Override to support editing the table view.
    override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {
        if editingStyle == .delete {
            // Delete the row from the data source
            //삭제를 하는 소스코드입니다.
            items.remove(at: (indexPath as NSIndexPath).row)
            itemsImageFile.remove(at: (indexPath as NSIndexPath).row)
            tableView.deleteRows(at: [indexPath], with: .fade)
        } else if editingStyle == .insert {
            // Create a new instance of the appropriate class, insert it into the array, and add a new row to the table view
        }    
    }
    
    // MARK: - Navigation

    // In a storyboard-based application, you will often want to do a little preparation before navigation
    //세그웨이를 이용한 뷰 전환
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // Get the new view controller using segue.destination.
        // Pass the selected object to the new view controller.
        if segue.identifier == "sgDetail" {
            let cell = sender as!  UITableViewCell
            let indexPath = self.tvListView.indexPath(for: cell)
            let detailView = segue.destination as! DetailViewController
            detailView.receiveItem(items[((indexPath! as NSIndexPath).row)])
        }
    }
}
```

리스트를 생성하는 부분의 코드입니다.

중요 부분은 여기인 것 같습니다.

```swift
    @IBAction func btnAddItem(_ sender: UIButton) {
        items.append(tfAddItem.text!)
        itemsImageFile.append(selectItemImage)
        tfAddItem.text = ""
        //루트뷰 컨트롤러 -> 테이블 뷰로 돌아갑니다.
        _ = navigationController?.popViewController(animated: true)
    }
```

DetailView에서는 이전에 네이게이션 뷰에서의 화면의 데이터 전송을 이용해서 데이터를 전송 받으면 됩니다.