# golang-tip-tricks

Golang hay còn được gọi là Go là ngôn ngữ lập trình được phát triển bởi các nhân viên của Google. Tới nay, Golang đã ra đời được hơn 10 năm (Từ năm 2007 tới nay). Sau khi ra đời tới nay, Go là một trong những ngôn ngữ lập trình quan trọng nhất trên thế giới. Go đơn giản là dễ học, dễ hiểu đối với tất các các nhà lập trình khác. Cú pháp của GO thì khá tương đồng với C. Một số ưu điểm nổi bật của GO có thể nói đến như:

- Tiện lợi : Go được so sánh với nhiều ngôn ngữ scripting language như Python, với khả năng đáp ứng nhiều nhu cầu lập trình phổ biến. Go cũng cung cấp khả năng quản lý bộ nhớ tự động bao gồm việc dọn dẹp rác. Ưu điểm đặc biệt của Go so với Python là, Go biên dịch code ra nhị phân một cách nhanh chóng. Và cũng khác hoàn toàn so với C++ Go biên dịch cực nhanh, khiến cho bản cảm tưởng khi làm việc với Go là bạn đang làm với 1 ngôn ngữ kịch bản, chứ k phải 1 ngôn ngữ biên dịch

- Tốc độ : Run nhị phân chậm hơn so với C, nhưng tốc độ này là không đáng kể so với hầu hết các ứng dụng. Hiệu suất của Go tốt ngang với C trong các công việc, và nói chung là nhanh hơn hẳn so với các ngôn ngữ nổi tiếng về tốc độ khác như Javascript, Ruby, Python

- Linh hoạt : Các file thực thi của Go có thể hoạt động độc lập mà không cần external dependencies mặc định. 

- Hỗ trợ : Toolchain của Go có sẵn dưới dạng library của Linux, MacOs hoặc windown, hay như là một container trong docker. Nhiều bản phát hành của Linux, Go được cài đặt mặc định trong đó. Go cũng hỗ trợ mạnh mẽ cho nhiều nền tảng developer.

Nói tóm lại, Golang đang và sẽ là xu thế của thế giới hiện nay, hay là trong một vài năm tới đây, nếu các bạn là một developer, dù có là người mới, hay là dev lâu năm trong những ngôn ngữ như PHP, Java, Ruby, Javascript, ... Thì tôi khuyên chân thành, chúng ta cũng hãy học và nghiên cứu Golang ngay từ hôm nay. Vì không ai tiết trước được công nghệ sẽ thay đổi thế nào trong ngày  mai. 

Cũng như bất kỳ một ngôn ngữ lập trình nào khác, khi mới bắt đầu tìm hiểu về nó. Ngoài việc đọc tutorial cùa ngôn ngữ đó ra, tôi thường quan tâm tới các tip and tricks của ngôn ngữ lập trình đó. Golang cũng không ngoại lệ, trong bài viết này, tôi sẽ giới thiệu tới các bạn đọc các tip & tricks trong ngôn ngữ Golang, hy vọng với những điều sau đây, sẽ giúp các bạn dễ dàng học Golang hơn. 

## 1. Thao tác với _map_

Trong code của bạn có khai báo một _map_, và bạn cần hiển thị nội dung của _map_ đang có. Cách phổ biến nhất để làm điều này đó là sử dụng hàm _range_ duyệt qua toàn bộ các phần tử của map và dùng hàm _fmt.Println()_ để in từng giá trị đó ra theo keys/values.

```
func main() {
  m := map[int]bool{
    1: true,
    5: true,
    99: true,
    101: false,
    14: true,
  }

  for k, v := range m {
    fmt.Printf("%d: %t\n", k, v)
  }
}
```

Ít hay nhiều thì người ta cũng phải đồng ý rằng, việc dựa vào các map có thứ tự là ý tưởng tồi, bởi vì chúng ta đang phụ thuộc vào một chức năng không phải mục đích chính của cấu trúc dự liệu đó. Golang đồng ý với điều này và thực hiện nó một cách quyết liệt. Golang sắp xếp lại thứ tự của map random dựa vào việc random seed trong mỗi lần chạy. Hãy thử đoạn code bên trên với (Go Playground)[https://play.golang.org/p/8IdNVrPsleU], trong này, tôi đã để sẵn hàm rand.Seed(1) trước khi chạy chương trình. Hãy thử chạy và xem kết quả, với lần 2, hay thay đổi giá trị trong hà seed, bạn sẽ nhận thấy thứ tự tin ra các phần từ của map được thay đổi. Seed thay đổi với mỗi lần go run, với Go playground, giá trị seed không thay đổi trong mỗi lần chạy, nhưng với môi trường production thì nó sẽ tự động thay đổi, Đây có thể nói là một điểm cộng của ngôn ngữ Go.

## 2. Struct trong Go

Để biết cách Go làm việc với struct như thế nào, chúng ta xem 2 struct User và Admin sau đây

```
type User struct {
  Name string
}

type Admin struct {
  User User
  Permissions map[string]string
}
```

Trong ví dụ trên, chúng ta có 2 đối tượng là User và Admin, trong đó Admin có thuộc tính là User cùng với Permissions. Nếu bạn muốn truy vấn tới tên của Admin, thì bạn cần gọi _.User.Name_

Tuy nhiên, hãy xem xét việc xóa một từ trong ví dụ trước, điều này sẽ làm thay đổi toàn bộ phương thức của đối tượng Admin:

```
type User struct {
  Name string
}

type Admin struct {
  User
  Permissions map[string]string
}
```

Bạn có để ý tới điều này, thuộc tính User đã được bỏ đi. Điều này có nghĩa là bạn có thể tha chiếu tới tên của Admin thông qua phương thức, admin.Name, không cần gọi trung gian thông qua User. Việc này giúp cho code của chúng ta trở nên gọn gàng hơn, nó chắc chắn sẽ được cho share methods và việc giữ cho code của bạn "clean"

Một cách sử dụng bổ sung khác của kiểu này được gọi là "Composing", bao gồm việc sử dụng nhiều kiểu khác nhau để tạo ra một kiểu mới hoàn toàn. Một ví dụ điển hình đó là  thư viện Read/Write

```
type ReadWriter interface {
    Reader
    Writer
}
```
Chúng ta có thể thấy từ định nghĩa trên là với interface ReadWriter, chúng ta có thể gọi tất các các methods được định nghĩa trong các class Read + Write, được định nghĩa ở những nơi khác. Biết được cấu trúc trong Go, điều này cực kỳ ý nghĩa, nó cũng cho phép Read và Write hoạt động độc lập với nhau, điều này là một điểm cộng. 

## Testing

Golang cũng cấp cho chúng ta một package để test đó là _testing_. Cung cấp các phương thức để chạy các bộ thử nghiệm. Các cờ -v, -short được xử dụng với _go test_ để hooked vào chính bản thân nó, người sử dụng có thể tự định nghĩa các hàm test của mình tùy thuộc vào mục đích nếu các cờ trên xuất hiện. 

Một ví dụ của việc viết test trong Go

```
import "fmt"

func main() {
  return fmt.Println(helloWorld())
}

func helloWorld() string{
  return fmt.Sprintf("Hello world")
}

import (
  "testing"
  "fmt"
)

func TestHelloWorld(t *testing.T) {
  if testing.Short() {
    t.Skip("Short test suite, skipping test.")
  }

  if testing.Verbose() {
    fmt.Println("About to test helloWorld()...")
  }

  exp := "Hello world"
  got := helloWorld()
  if exp != got {
    t.Errorf("Expected %s, got %s", exp, got)
  }
}
```

Trong ví dụ trên, bạn có thể nhìn thấy, package testing đã expose ra 2 phương thức test đó là testing.Short() và testing.Verbose(), có thể dùng để  testing logic, bạn có thể xử dụng chúng như sau:

```
go test -short
go test -v

```

Mặc dù đáy chỉ là tính năng nhỏ, nhưng nó cho phép các lập trình viên kiểm soát rõ ràng hơn với bộ test của họ


