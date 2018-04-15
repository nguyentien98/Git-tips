# 10 lời khuyên giúp nâng tầm kỹ năng sử dụng Git của bạn

Gần đây, chúng tôi đã phát hành một vài bài hướng dẫn nhằm giúp bạn làm quen với Git cơ bản và sử dụng Git trong môi trường nhóm. Các câu lệnh mà chúng tôi thảo luận khá đủ để giúp một nhà phát triển sống sốt trong thế giới Git. Trong bài đăng này, chúng tôi sẽ cố tìm hiểu làm cách nào để quản lý thời gian của bạn một cách hiệu quả và sử dụng đầy đủ các tính nắng mà Git cung cấp.

Lưu ý: Một vài câu lệnh trong bài viết này bao gồm một phần của câu lệnh nằm trong ngoặc vuông ( ví dụ git add -p [file_name]). Trong những ví dụ đó, bạn sẽ thêm các số, định danh cần thiết, ... mà bỏ qua dấu ngoặc vuông.

## 1. Git tự hoàn thiện

Nếu bạn chạy các câu lệnh Git qua command line, đó sẽ là một công việc mệt mỏi khi mỗi lần đều phải gõ các câu lệnh bằng tay. Để giúp bạn với điều này, bạn có thể bật chức năng tự hoàn thiện của câu lệnh Git trong vài phút.

Để lấy script, chạy lệnh sau trong hệ thống Unix:

```
cd ~
curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```

Tiếp đó thêm dòng này vào file ```~/.bash_profile``` của bạn:

```
if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
fi
```

Mặc dù tôi đã đề cập đến điều này trước đây, tôi vẫn phải nhắc lại rằng: Nếu bạn muốn sử dụng các tính năng của Git một cách đầy đủ, bạn nên chuyển sang sử dụng giao diện trên command line.

## 2. Loại bỏ các file trong Git

Bạn có cảm thấy mệt mỏi khi các file biên dịch (như ```.pyc```) xuất hiện trong repository Git của bạn? Hay bạn cảm thấy chán ngấy rằng bạn đã thêm chúng vào Git? Không cần nhìn đâu xa, có một cách mà qua đó bạn có thể bảo Git loại bỏ những file cũng như các thư mục mà ta mong muốn. Chỉ cần đơn giản tạo 1 file với tên ```.gitignore``` và liệt kệ các file và thử mục bạn không muốn Git theo dõi. Bạn có thể tạo những ngoại lệ bằng cách sử dung dấu chấm than (!).

```
*.pyc
*.exe
my_db_config/

!main.pyc
```

## 3.  Ai đã làm rối code của tôi?

Bản năng tự nhiên của con người là trách móc người khác khi có gì đó bị sai. Nếu server đang chạy của bạn hỏng, sẽ rất dễ dàng để tìm ra thủ phạm - chỉ cần thực hiện ```git blame```. Câu lệnh này sẽ cho bạn thấy tác giả của mỗi dòng trong một file, commit thực hiện thay đổi cuối cùng của dòng đó, và mốc thời gian của commit. 

```
git blame [file_name]
```

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946443git-ninja-01.png)

và như ảnh chụp màn hình dưới đây, bạn có thể thấy cách mà câu lệnh này theo dõi một repository lớn hơn

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946441git-ninja-02.png)

## 4. Xem lại lịch sử của Repository

Hay xem qua cách sử dụng của ```git log``` trong bài hướng dẫn trước đây, truy nhiên, có 3 lựa chọn mà bạn nên biết.

* ```--oneline``` – Nén thống tin hiện thỉ bên cạnh mỗi commit thành các commit hash giảm thiểu và các thông điệp commit, tất cả được hiển thị trong một dòng đơn.
* ```--graph``` –  Lựa chọn này vẽ một miêu tả đồ thị dựa trên văn bản về lịch sử trên phía tay trái của đầu ra. Nó sẽ vỗ nghĩa nếu bạn đang xem lịch sử cho một nhánh đơn.
* ```--all``` – Hiển thị lịch sử của tất cả các nhánh.

Ví dụ khi kết hợp các lựa chọn:
![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946444git-ninja-03.png)

## 5. Không bao giờ theo dõi sót commit nào

Ví dụ bạn đã commit cái gì đó mà bạn không muốn và cuối cùng bạn thực hiện việc hard reset để quay trở lại trạng thái trước đó. Sau đó, bạn nhận ra bạn đã bỏ sót vài thông tin khác trong tiến trình và muốn lấy nó lại, hay ít nhất là xem nó. Đây là lúc ```git reflog``` có thể giúp bạn.

```git log``` cho bạn biết về commit gần nhất, cha của nó, cha của cha nó,... Tuy nhiên, ```git reflog``` là một danh sách các commit mà head đã trỏ tới. Hãy nhớ rằng nó cục bộ với hệ thống của bạn, nó không phải là một phần của repository của bạn và không được bao gồm trong push hay merge.

Nếu tôi chạy ```git log```, tôi sẽ lấy được các commit là một phần của repository của tôi:

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png)

Tuy nhiên, một ```git reflog``` hiển thị 1 commit (```b1b0ee9 - HEAD@{4}```) mà bị mất khi bạn thực hiện một hard reset.

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png)

## 6. Stage các phần của các file thay đổi cho Commit 

Đây nhìn chung là cách làm tốt để tạo các commit hướng chức năng, trong đó, mỗi commit phải đại diện cho một tính năng hoặc một việc sửa lỗi. Tưởng tượng điều gì sẽ xảy ra nếu bạn sửa hai lỗi, hoặc thêm nhiều tính năng mà không commit sự thay đổi. Trong trường hợp này, bạn có thể để các sự thay đổi trong 1 commit đơn lẻ. Nhưng có một cách khác tốt hơn: stage các file riêng lẻ và commit chúng lần lượt.

Ví dụ bạn đã tạo nhiều thay đổi cho một file đơn lẻ và muốn chúng xuất hiện trong các commit riêng. Trong trường hợp này, chúng ta sẽ thêm các file với tiền tố ```-p``` trong câu lệnh add.
 
```
git add -p [file_name]
```

Hãy cố gắng thực hiện điều tương tự. Tôi đã thêm 3 dòng mới cho ```file_name``` và tôi chỉ muốn dòng thứ nhất và thứ ba xuất hiện trong commit của tôi. Hay xem những gì ```git diff``` hiện thị cho chúng ta.

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png)

Và hãy xem điều gì xảy ra khi chúng ta thềm tiền tố ```-p``` cho câu lệnh ```add``` của chúng ta.
![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png)

Có vẻ như Git giả định rằng tất cả sự thay đổi là một phần của ý tưởng tương tự nhau, vì thế Git nhóm chúng vào nhóm lớn đơn lẻ. Bạn có những lựa chọn sau đây:

* Gõ y để stage nhóm đó
* Gõ n để không stage nhóm đó
* Gõ e để sửa nhóm đó thủ công
* Gõ d để thoát và đến file tiếp theo
* Gõ s đẻ chia nhỏ nhóm

Trong trường hợp của chúng ta, chúng ta chắc chắn muốn chia nó thành các phần nhỏ hơn để tùy chọn thêm hoặc bỏ qua phần còn lại.

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png)

Như bạn có thể thấy, chúng ta đã thêm dòng đầu và dòng thử ba và loại bỏ dòng thứ hai. Bạn có thể sau đó xem trạng thái của repository và tạo một commit.

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png)

## 7. Ép nhiều commit

Khi bạn gửi code của bạn để được xem xét lại và tạo một pull request(điều thường xảy ra trong các dự án mã nguồn mở), bạn có thể sẽ phải yêu cầu tạo ra một sự thay đổi trong code của bạn trước khi nó được chấp nhận. Bạn tạo ra sự thay đổi, chỉ khi được yêu cầu và thực hiện lần nữa trong lần xem xét lại tiếp theo. Trước khi bạn biết nó, bạn có vài commit thêm. Lý tưởng nhất là bạn có thể ép chúng lại làm một và sử dụng câu lệnh ```rebase```.

```
git rebase -i HEAD~[number_of_commits]
```

Nếu bạn muốn ép hai commit cuối, câu lệnh bạn chạy như sau:

```
git rebase -i HEAD~2
```

Khi chạy câu lệnh này, bạn sẽ được dẫn tới một giao diện tương tác nơi liệt kê các commit và sẽ hỏi bạn chọn cái nào để ép. Lý tưởng nhất là bạn ``` chọn ``` commit cuối cùng và ``` ép ``` với các cái cũ.

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png)

Sau đó bạn sẽ được yêu cầu cung cấp nội dung commit cho commit mới. Tiến trình này về bản chất sẽ ghi lại lịch sử commit.

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png)

## 8. Cất những thay đổi chưa được commit

Ví dụ bạn đang làm việc trên một tính năng hay một lỗi cụ thể, và bạn bỗng nhiên được yêu cầu mô tả công việc của bạn. Công việc hiện tại của bạn không đủ hoàn thiện để được commit, và bạn không thể đưa ra nhưng mô tả tại stage này ( bạn cũng không trở về các thay đổi). Trong trường hợp này, ```git stash``` sẽ giải cứu bạn. Stash bản chất lấy tất cả các thay đổi của bạn và lưu trữ chúng để sử dụng sau này. Để cất những thay đổi của bạn, bạn chỉ đơn giản chạy lệnh sau.

```
git stash
```

Để check danh sách các stash, bạn có thể chạy lệnh sau:

```
git stash list
```

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png)

Nếu bạn muốn bỏ cất và khôi phục những thay đổi chưa commit, bạn gọi stash:

```
git stash apply
```

Trong ảnh chụp màn hình cuối cùng, bạn có thể thấy rằng mỗi stash có một định danh, một số duy nhất (mặc dù chúng ta chỉ có duy nhất một stash trong trường hợp này). Trong trường hợp bạn muốn gọi chỉ duy nhất stash được chọn, bạn thêm định đặc tả cho câu lệnh ```apply```
```
git stash apply stash@{2}
```

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png)


## 9. Kiểm tra những commit bị mất

Mặc dù ```reflog``` là một cách để kiểm tra các commit bị mất, nó không thích hợp trong các repository lớn. Đây là khi câu lệnh ```fsck``` (kiểm tra hệ thống file) sẽ giúp ích.

```
git fsck --lost-found
```

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png)

Ở đây bạn có thể thấy các commit bị mất. Bạn có thể kiểm tra các thay đổi trong commit bằng cách chạy ```git show [commit_hash]``` hoặc khôi phục nó bằng cách chạy ```git merge [commit_hash]```.
```git fsck``` có lợi nhiều hơn ```reflog```. Ví dụ bạn đã xóa một branch remote và sau đó clone repository. Với ```fsck``` bạn có thể tìm kiếm và khôi phục các nhánh remote đã bị xóa.


## 10. Cherry Pick

Cuối cùng là câu lệnh Git tinh tế nhất. Câu lệnh ```cherry-pick``` là câu lệnh Git ưa thích nhất của tôi, bởi vì ý nghĩa thực tế cũng như tính hữu dụng của nó!

Đơn gỉan ```cherry-pick``` sẽ chọn một commit đơn lẻ từ các nhánh khác nhau và hợp chúng với cái hiện tại. Nếu bạn đang làm việc theo cách song song trên hai nhánh hoặc nhiều hơn, bạn có thể thấy một lỗi mà xuất hiện ở tất cả các nhánh. Nếu bạn giải quyết nó trong một nhánh, bạn có thể cherry pick commit đến các nhánh khác, mà không làm ảnh hưởng tới với các file hay commit khác.


Hãy xem xét một ví dụ khi chúng ta có thể sử dụng điều này. Tôi có 2 nhánh và tôi muốn ```cherry-pick``` commit ```b20fd14: Cleaned junk``` đến 1 nhánh khác

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png)

Ta chuyển tới nhánh mà mong muốn ```cherry-pick``` commit, và chạy lệnh sau

```
git cherry-pick [commit_hash]
```

![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png)

Mặc dù lần này ta đã có một ```cherry-pick``` rất ổn và ngăn nắp, bạn nên biết rằng câu lệnh này thường dẫn tới các xung đột, vì vậy sử dụng nó cẩn thận.

## Kết luận

Với những điều trên, chúng ta đi đến phần kết thúc trong danh sách các lời khuyên mà tôi nghĩ rằng có thể giúp nâng cao kĩ năng sử dụng Git của ban. Git là thứ tốt nhất và nó có thể  thực hiện bất cứ điều gì mà bạn có thể tưởng tượng. Vì thế, hãy luốn cố gắng thách thức bản thân với Git, khả năng lớn là bạn sẽ học được nhiều điều mới mẻ!






