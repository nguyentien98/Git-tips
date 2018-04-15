#10 lời khuyên giúp nâng các kĩ năng git của bạn lên tầm cao mới

Gần đây, chúng tôi đã phát hành 1 vài hướng dẫn nhằm giúp bạn làm quen với Git cơ bản và sử dụng Git trong môi trường nhóm. Các câu lệnh mà chúng tôi thảo luận khá là đủ để giúp 1 người phát triển sống sốt trong thế giới Git. Trong bài đăng này, chúng tôi sẽ cố tìm hiểu làm cách nào để quản lý thời gian của bạn 1 cách hiệu quả và sử dụng đầy đủ các tính nắng mà Git cung cấp.

Lưu ý: 1 vài câu lệnh trong bài viết này bao gồm 1 phần của câu lệnh nằm trong ngoặc vuông ( ví dụ git add -p [file_name]). Trong những ví dụ đó, bạn sẽ thêm các số, định danh cần thiết, vân vân mà không có ngoặc vuông.

##1. Git tự hoàn thiện

Nếu bạn chạy các câu lệnh Git qua command line, sẽ là 1 công việc mệt mỏi mỗi lần gõ các câu lệnh bằng tay. Để giúp bạn với điều này, bạn có thể bật chức năng tự hoàn thiện của câu lệnh Git trong vài phút.

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

Mặc dù tôi đã đề cập đến điều này trước đây, tôi đã không thể nhấn mạnh đủ. Nếu bạn muốn sử dụng tính năng này của Git 1 cách đầy đủ, bạn nên định nghĩa sự thay đổi cho giao diện command line.

##2. Loại bỏ các file trong Git

Bạn có cảm thấy mệt mỏi khi biên dịch các file (như ```.pyc```) xuất hiện trong repository Git của bạn? Hay bạn cảm thấy chán ngấy rằng bạn đã thêm cho cho Git? Không cần nhìn đâu xa, có 1 cách mà qua đó bạn có thể bảo Git loại bỏ những file cũng như thư mục cụ thể. Chỉ cần đơn giản tạo 1 file với tên ```.gitignore``` và liệt kệ các file và thử mục bạn không muốn Git theo dỗi. Bạn có thể tạo những ngoại lệ sử dung dấu chấm than (!).

```
*.pyc
*.exe
my_db_config/

!main.pyc
```

##3.  Ai đã làm rỗi code của tôi?

Đấy là bản năng tự nhiên của con người khi trách móc người khác khi có gì đó bị sai. Nếu server sản xuất của bạn hỏng, sẽ rất dễ để tìm ra thủ phạm - chỉ cần thực hiện ```git blame```. Câu lệnh này sẽ cho bạn thấy tác giả của mỗi dòng trong 1 file, commit thực hiện thay đổi cuối cùng của dòng đó, và mốc thời gian của commit. 

```
git blame [file_name]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946443git-ninja-01.png)

và như chụp màn hình dưới đây, bạn có thể thấy cách mà câu lệnh này theo dôi 1 repository lớn hơn

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946441git-ninja-02.png)

##4. Xem lại lịch sử của Repository

Hay xem qua cách sử dụng của ```git log``` trong bài hướng dẫn trước đây, truy nhiên, có 3 lựa chọn mà bạn nên biết.

* ```--oneline``` – Nén thống tin hiện thỉ bên cạnh mỗi commit thành các commit hash giảm thiểu và các thông điệp commit, tất cả được hiển thị trong 1 dòng đơn
* ```--graph``` –  Lựa chọn này vẽ 1 miêu tả đồ thị dựa trên văn bản về lịch sử trên phía tay trái của đầu ra. Nó sẽ vỗ nghĩa nếu bạn đang xem lịch sử cho 1 nhánh đơn
* ```--all``` – Hiển thị lịch sử của tất cả các nhánh

Đấy khi kết hợp các lựa chọn thì nó trông thế này:
![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946444git-ninja-03.png)

##5. Không bao giờ theo dỗi sót 1 commit

Hãy nói rằng bạn đã commit cái gì đó mà bạn không muốn và kết thúc bằng việc thực hiện hard reset để quay trở lại trạng thái trướcước. Gần đây, bạn nhận ra bạn đã sót vài thông tin khác trong tiến trình và muốn lấy nó lại, hay ít nhất là xem nó. Đây là lúc ```git reflog``` có thể giúp bạn.

```git log`` đơn giản cho bạn biết commit cuối cuối, cha của nó, cha của cha nó, và vân vân. Tuy nhiên, ```git reflog``` là 1 danh sách các commit mà head đã trỏ tới. Hãy nhớ rằng nó cục bộ với hệ thống của bạn, nó không phải là 1 phần của repository của bạn và không bao gồm các push hay merge.

Nếu tôi chạy ```git log```, tôi sẽ lấy được các commit là 1 phần của repository của tôi

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png)

Tuy nhiên, 1 ```git reflog``` hiển thị 1 commit (```b1b0ee9 - HEAD@{4}```) mà bị mất khi bạn thực hiện 1 hard reset.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png)

##6. Stage 1 phần của các file thay đổi cho Commit 

Đây nhìn chung là cách làm tốt để tạo các commit hướng chức năng, trong đó, mỗi commit phải đại diện cho 1 tính năng hoặc 1 sửa lỗi. Tưởng tượng cái gì sẽ xảy ra nếu bạn sửa 2 lỗi, hoặc thêm nhiều tính năng mà không commit sự thay đổi. Trong trường hợp này, bạn có thể để các sự thay đổi trong 1 commit đơn le. Nhưng có 1 cách khác tốt hơn: stage các file riêng lẻ và commit chúng lần lượt.

Hãy nói rằng bạn đã tạo nhiều thay đổi cho 1 file đơn lẻ và muốn chúng xuất hiện trong các commit riêng. Trong trường hợp này, chúng tôi sẽ thêm các file với tiền tố ```-p``` đến câu lệnh add của chúng tôi.
 
```

git add -p [file_name]

```

Hãy cố gắng mô tả điều tương tự. Tôi đã thêm 3 dòng mới cho ```file_name``` và tôi chỉ muốn dòng thứ nhất và thứ ba xuất hiện trong commit của tôi. Hay xem những gì ```git diff``` hiện thị cho chúng ta.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png)

Và hãy xem điều gì xảy ra khi chúng ta thềm tiền tố ```-p``` cho câu lệnh ```add``` của chúng ta.
![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png)

Trông như rằng Git giả định rằng tất cả sự thay đổi là 1 phần của ý tưởng tương tự, vì thế nhóm chúng vào nhóm lớn đơn lẻ. Bạn có những lựa chọn sau đây:

* Gõ y để stage nhóm đó
* Gõ n để không stage nhóm đó
* Gõ e để sửa nhóm đó thủ công
* Gõ đ để thoát và đến file tiếp theo
* Gõ s đẻ chia nhỏ nhóm

Trong trường hợp của chúng tôi, chúng tôi chắc chắn muốn chia nó thành các phần nhỏ hơn để thêm 1 cách có lựa chọn 1 vài chỗ và loại bỏ phần còn lịa. 
![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png)

Như bạn có thể thấy, chúng tôi phỉa thêm dòng đầu và dòng thử 3 và loại bỏ dòng thứ 2. Bạn có thể sau đó xem trạng thái của repository và tạo 1 commit.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png)

##7. Ép nhiều commit

Khi bạn gửi code của bạn để xem lại và tạo 1 pull request( thử xảy ra thường xuyên trong các dự án open source), ạnạn có thể sẽ được yêu cầu để tạo 1 thay đổi cho code của bạn trước khi nó được chấp nhaank. Bạn tạo sự thay đổi, chỉ khi được yêu cầu thay đổi nó 1 lần nữa trong lần xem lại tiếp theo. Trước khi bạn biết nó, bạn có vài commit thêm. Lý tưởng nhất là bạn có thể ép chúng lại làm 1 sử dụng câu lệnh ```rebase```.

```

git rebase -i HEAD~[number_of_commits]

```

Nếu bạn muốn ép 2 commit cuối, câu lệnh bạn chạy như sau:
```

git rebase -i HEAD~2

```

Khi chạy câu lệnh này, bạn sẽ được dẫn tới 1 giao diện tương tác liệt kê các commit và hỏi bạn cái nào để ép. Lý tưởng nhất là bạn ``` chọn``` commit cuối cùng và ``` ép ``` với các cái cũ.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png)

Sau đó bạn sẽ được gỏi cung cấp thông điệp commit cho commit mới. Tiến trình này về bản chất sẽ ghi lại lịch sử commit.
![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png)

##8. Cất những thay đổi chưa được commit

Hãy nói rằng bạn đang làm việc trên 1 tính năng hay 1 lỗi cụ thể, và bạn bỗng nhiên được yêu cầu mô tả công việc của bạn. Công việc hiện tại của bạn không hoàn thiện đủ để được commit, và bạn không thể đưa ra nhưng mô tả tại stage này( mà không trở về các thay đổi). Trong trường hợp này, ```git stash``` tới để giải cứu bạn. Stash bản chất lấy tất cả các thay đổi của bạn và lưu trữ chúng để sử dụng sau này. Để cất những thay đổi của bạn, bạn chỉ đơn giản chạy lệnh sau.

```

git stash

```

Để check danh sách các stash, bạn có thể chạy lệnh sau:

```

git stash list

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png)

Nếu bạn muốn bỏ cất và khôi phục nhưng thay đổi những thay đổi chưa commit, bạn gọi stash:
```

git stash apply

```

Trong màn hình chụp cuối cùng, bạn có thể thấy rằng mỗi stash có 1 định danh, 1 số duy nhất (mặc dù chúng tôichỉ có duy nhất 1 stash trong trường hợp này). Trong trường hợp bạn muốn gọi chỉ duy nhất stash được chọn., bạn thêm định đặc tả cho câu lệnh ```apply```
```

git stash apply stash@{2}

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png)


##9. Kiểm tra những commit bị mất

Mặc dù ```reflog``` là 1 cách để kiểm tra các commit bị mất, nó không tiện trong các repository lớn. Đó là khi câu lệnh ```fsck``` (kiểm tra hệ thống file) vào cuộc

```

git fsck --lost-found

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png)

Ở đây bạn có thể thấy các commit bị mất. Bạn có thể kiểm tra các thay đổi trong commit bằng cách chạy ```git show [commit_hash]``` hoặc khôi phục nó bằng cách chạy ```git merge [commit_hash]```.
```git fsck``` có 1 lợi thế hơn ```reflog```. Hãy nói rằng bạn đã xóa 1 branch remote và sau đó clone repository. Với ```fsck``` bạn có thể tìm kiếm và khôi phục các nhánh remote bị xóa.


##10. Cherry Pick

Cuối cùng tôi đã lưu lại các câu lệnh Git tao nhã nhất. Câu lệnh ```cherry-pick` là câu lệnh Git ưa thích nhất của tôi, bởi vì ý nghĩa thực cũng như tính hữu dụng của nó!

Với những giới hạn đơn giản nhất, ```cherry-pick``` sẽ chọn 1 commit đơn lẻ từ các nhánh khác nhau và hợp chúng với cái hiện tại. Nếu bạn đang làm việc theo cách song sóng trên 2 hay nhiều hơn nhánh, bạn có thể chú ý 1 lỗi mà xuất hiện ở tất cả các nhánh. Nếu bạn giải quyết nó trong 1, bạn có thể cherry pick commit đến cácnhánh khác, mà không làm rỗi loạn với các file hay commit khác


Hãy hình dung 1 kịch bản khi bạn có thể gọi nó. Tôi có 2 nhánh và tôi muốn ```cherry-pick``` commi ts```b20fd14: Cleaned junk``` đến 1 nhánh khác
![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png)

Tôi chuyển tới nhánh tới nhánh tôi muốn ```cherry-pick``` commit, và chạy lệnh sau
```

git cherry-pick [commit_hash]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png)

Mặc dù lần này tôi đã dọn ```cherry-pick```, bạn nên biết rằng cây lệnh này thường dẫn tới các xung đột, vì vậy sử dụng nó cẩn thận.

##Kết luận

Với những điều này, chúng tôi đi đến kết luận của danh sách các lời khuyên của chúng tôi mà tôi nghĩ rằng có thể giúp bạn nâng tầm các kĩ năng Git của ban. Git là thứ tốt nhất ngoài đó và nó có thể  hoàn thành bất cứ thứ gì mà bạn có thể tưởng tượng. Vìì thế, hãy luốn cố gắng thách thức bản thân với Git. Cơ hội đến, bạn sẽ có thể học được điều gì đó mới mẻ!





