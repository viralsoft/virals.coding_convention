## Portalbeanz Git flow

Flow tham khảo: http://nvie.com/posts/a-successful-git-branching-model/

### Giả định
* Đã tạo PBVN Repository (Nguồn trung tâm) trên Bitbucket（hiện tại công ty đang dùng bitbucket）。
* Branch mặc định của PBVN Repository là master。
* Lập trình viên có thể  fork (tạo nhánh) đối với PBVN Repository。
* Đã quyết định người review và người có quyền merge (hiện tại dev nào cũng có quyền)。

### Nguyên tắc
* Mỗi pull-request tương ứng với một ticket và chỉ có một commit trong đó。
* Nội dung của commit là `refs [Loại ticket] #[Số ticket] [Nội dung ticket]` （Ví dụ: `refs bug #1234 update order`）。
* Tại môi trường local(trên máy lập trình viên), tuyệt đối không được thay đổi code khi ở branch master。Nhất định phải thao tác trên branch khởi tạo để làm task。

### Chuẩn bị

    1. Trên Github (Bitbucket), fork PBVN Repository về tài khoản của mình（repository ở tài khoản của mình sẽ được gọi là Forked Repository）。

    2. Clone (tạo bản sao) Forked Repository ở môi trường local。Tại thời điểm này, Forked Repository sẽ được tự động đăng ký dưới tên là `origin`。
        $ git clone [URL của Forked Repository]

    3. Truy cập vào thư mục đã được tạo ra sau khi clone, đăng ký PBVN Repository dưới tên `upstream`。
        $ cd [thư mục được tạo ra]
        $ git remote add upstream [URL của PBVN Repository]

  ### Lưu ý:
    Nếu được thao tác trực tiếp trên Repository thì origin ~ upstream
### Quy trình

Từ đây, PBVN Repository và Forked Repository sẽ được gọi lần lượt là `upstream` và `origin`。
     ### Lưu ý:
        Nếu được thao tác trực tiếp trên Repository thì origin ~ upstream
1. Đồng bộ hóa branch master tại local với upstream。
    $ git checkout master
    $ git pull upstream master

2. Tạo branch để làm task từ branch master ở local. Tên branch là số ticket của task（Ví dụ: `task/1234`）。
    $ git checkout master # <--- Không cần thiết nếu đang ở trên branch master
    $ git checkout -b task/1234

3. Tiến hành làm task（Có thể commit bao nhiêu tùy ý）。
   *** có thể  bỏ qua bước 4 nếu được phép có nhiều commit.
4. Trường hợp đã tạo nhiều commit trong quá trình làm task trước khi push phải dùng rebase -i để hợp các commit lại thành 1 commit duy nhất。
    $ git rebase -i [mã hash của commit trước commit đầu tiên trong quá trình làm task]
        =>  pick 07c5abd Init
            pick fe7b2ab test1
            pick de9b1eb test2
    4.1   thay "s" (squard) vào "pick" từ commit thứ 2 trở xuống.
        =>
            pick 07c5abd Init
            s fe7b2ab test1
            s de9b1eb test2

    4.2 nó sẽ hiện lại commit message ở edittor kiểu như này
        # This is the 2nd commit message:
        test1
        # The 3rd commit message will be skipped:
        test2
        => ta có thể bỏ hết mesage đi bằng # vào đầu dòng hoặc xoá đi
        thêm commit của mình vào => save và thoát edittor

5. Quay trở về branch master ở local và lấy code mới nhất về
    $ git checkout master
    $ git pull upstream master
6. Quay trở lại branch làm task, sau đó rebase với branch master。
    $ git checkout task/1234
    $ git rebase master hoặc $ git merge master --no-ff
    **Trường hợp xảy ra conflict trong quá trình rebase hoặc merge、=> ngồi mà fix, gọi thêm ng cùng làm task trước đó.

7. Push code lên origin。
    $ git push origin task/1234
8. Tại origin trên Github（Bitbucket）、từ branch `task/1234` đã được push lên hãy gửi pull-request đối với branch master của upstream.

9. Hãy gửi link URL của trang pull-request cho reviewer để tiến hành review code。

    9.1. Trong trường hợp reviewer có yêu cầu sửa chữa, hãy thực hiện các bước 3. 〜 6.。

    9.2.1 merge bằng "git rebase"
        * push -f (push đè hoàn toàn lên code cũ) đối với remote branch làm task -f làm thay đổi history code (rewrite)。
        * $ git push origin task/1234 -f
    9.2.1 merge bằng "git merge"
            $ git push origin task/1234
    9.3 Tiếp tục gửi lại URL cho reviewer để tiến hành việc review code。

10. Nếu reviewer đồng ý với pull-request, sẽ thực hiện việc merge pull-request。
11. Quay trở lại 1。






### Khi xảy ra conflict trong quá trình merge

    $ git rebase master
    First, rewinding head to replay your work on top of it...
    Applying: refs #1234 update order
    Using index info to reconstruct a base tree...
    Falling back to patching base and 3-way merge...
    Auto-merging path/to/conflicting/file
    CONFLICT (add/add): Merge conflict in path/to/conflicting/file
    Failed to merge in the changes.
    Patch failed at 0001 refs #1234 update order
    The copy of the patch that failed is found in:
        /path/to/working/dir/.git/rebase-apply/patch

    When you have resolved this problem, run "git rebase --continue".
    If you prefer to skip this patch, run "git rebase --skip" instead.
    To check out the original branch and stop rebasing, run "git rebase --abort".

    1. Hãy thực hiện fix lỗi conflict thủ công（những phần được bao bởi <<< và >>> ）。
        Trong trường hợp muốn dừng việc rebase lại 。
        $ git merge --abort
    2. Khi đã giải quyết được tất cả các conflict, tiếp tục thực hiện việc merge bằng:
        $ git add file hoặc git add .
        $ git commit -m "merge message"
        $ $ git push origin task/1234

### Khi xảy ra conflict trong quá trình rebase

Khi xảy ra conflict trong quá trình rebase, sẽ có hiển thị như dưới đây (tại thời điểm này sẽ bị tự động chuyển về một branch vô danh)
```sh
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: refs #1234 update order
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging path/to/conflicting/file
CONFLICT (add/add): Merge conflict in path/to/conflicting/file
Failed to merge in the changes.
Patch failed at 0001 refs #1234 update order
The copy of the patch that failed is found in:
    /path/to/working/dir/.git/rebase-apply/patch

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```

1. Hãy thực hiện fix lỗi conflict thủ công（những phần được bao bởi <<< và >>> ）。
    Trong trường hợp muốn dừng việc rebase lại, hãy dùng lệnh:
        $ git rebase --abor

2. Khi đã giải quyết được tất cả các conflict, tiếp tục thực hiện việc rebase bằng:

    ```sh
    $ git add .
    $ git rebase --continue
    ```

### một số lệnh git cơ bản nên tham khảo
$ git clone
$ git diff
$ git commit --am
$ git checkout
$ git checkout -b
$ git fetch
$ git show
$ git reset
$ git remote
$ git revert
$ git stash
$ git pop
$ git apply
$ git log