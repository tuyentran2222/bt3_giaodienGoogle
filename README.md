## Thông tin sinh viên
> Họ và tên: Trần Văn Tuyền

> Lớp: Công nghệ thông tin 03- K63

> Trường: Đại học Bách Khoa Hà Nội

<h1 align="center">
  Markdown Technical Report
</h1>

 # **Factory Method**📚📖📙
##  ☹️ Vấn đề
Hãy tưởng tượng bạn đang tạo một ứng dụng để quản lý dịch vụ vận chuyển. Phiên bản ban đầu chỉ có thể xử lý việc vận tải bằng xe tải và hầu hết phần mã đều nằm ở lớp `Truck 🚛`. Sau một thời gian, ứng dụng của bạn trở nên phổ biến, kèm theo đó là những yêu cầu từ các công ty vận tải đường biển về các dịch vụ hậu cần đường biển vào ứng dụng. Đến đây, trong đầu lại chợt mở ra một câu hỏi lớn: “Làm thế nào bây giờ, làm thế nào để thêm các dịch vụ hậu cần đường biển trong khi hầu hết các mã của bạn đều thêm vào lớp `Truck 🚛`? Việc thế, việc thêm `Ship🚢`  vào ứng dụng sẽ làm thay đổi với các đoạn mã cơ sở. Điều này làm cho đoạn mã của bạn thêm tương đối rối và khó chịu. Và hơn nữa, nếu sau này muốn mở rộng các dịch vụ hậu cần đường hàng không nữa thì càng khiến đoạn mã thêm phức tạp, khó theo dõi hơn…

## 🌞Giải pháp
Thay thế việc gọi hàm constructor trực tiếp bằng việc sử dụng một phương thức factory đặc biệt. Ta xây dựng một giao diện chung `Transport`, 2 lớp `Truck` và `Ship` thực thi giao diện đó. Lớp factory sẽ quản lý và trả về các đối tượng theo yêu cầu, giúp chi việc khởi tạo đối tượng `Truck` hay `Ship` linh hoạt hơn.
## 🔍  Định nghĩa

<p align="center">
  <img src="https://i.postimg.cc/ZRHdxHkW/factory.jpg">
  <p align="center">Hình 1: Minh họa cách hoạt động của Factory method.</p>
</p>

Factory Method là một mẫu thiết kế khởi tạo cung cấp một giao diện để tạo các đối tượng trong lớp cha, nhưng cho phép các lớp con thay đổi loại đối tượng sẽ được tạo. Factory Method giao việc khởi tạo một lớp cụ thể cho lớp con.

## ⚙ Cấu trúc

***Biểu đồ lớp***
<p align="center">
  <img src="https://i.postimg.cc/XvhShSwF/Factory.png">
  <p align="center">Hình 2: Biểu đồ lớp của Factory method.</p>
</p>

> Trong đó: 


> + `Product`: giao diện cho các đối tượng mà `phương thức factory` tạo ra.

> + `ConcreteProduct`: thực thi giao diện `Product`.

> + `Creator` ( còn được gọi là `Factory`): trả về một đối tượng của lớp.


> Tất cả các `Product` cụ thể đều là lớp con con của lớp `Product`.
Lớp `Creator` chỉ định các hành vi tiêu chuẩn và chung của các sản phẩm và khi cần một sản phẩm mới các chi tiết sẽ được cung cấp bởi các client  cung cấp cho `ConcreteCreator`.

***Biểu đồ trình tự***

<p align="center">
  <img src="https://i.postimg.cc/L4G7M3gd/Seque-Factory.png">
  <p align="center">Hình 3: Biểu đồ trình tự của Factory method.</p>
</p>

## 📚 Code minh họa.
Ta xây dựng một nhà máy để sản xuất bánh xe:
```
//abstract Product

public abstract class Wheel {
	public abstract float getDiameter();
	public abstract float getWidth();
	@Override
	public String toString() {
		return “Diameter = “+ String.valueOf(this.getDiameter())
		+ “Width = "+ String.valueOf(this.getWidth());
	}
}
//ConcreteProduct: ghi đè các phương thức tạo bánh xe.
//Bánh ô tô
public class CarWheel extends Wheel {
	private float diameter;
	private float width;
	
	public CarWheel(float diameter, float width) {
		this.diameter = diameter;
		this.width = width;
	}
	
	@Override
	public float getDiameter() {
		return diameter;
	}

	@Override
	public float getWidth() {
		return width;
	}
}

//Bánh xe đạp
public class BikeWheel extends Wheel {
	private float diameter;
	private float width;
	
	public BikeWheel(float diameter, float width) {
		this.diameter = diameter;
		this.width = width;
	}

	@Override
	public float getDiameter() {
		return diameter;
	}
	
	@Override
	public float getWidth() {
		return width;
	}
}

//interface factory
public interface WheelAbstractFactory{
	public Wheel getWheel();
}

public class WheelFactory implements WheelAbstractFactory {
	public static Wheel getWheel(String type, float diameter,float width) {
		if(“CarWheel”.equalsIgnoreCase(type)){
			return new CarWheel(diameter, width);
		}
		else if(“BikeWheel”.equalsIgnoreCase(type)){
			return new BikeWheel(diameter, width);
	}
	return null;
}
```
## ✅  Khả năng áp dụng.
- Sử dụng `Factory Method` khi bạn không biết trước loại và phụ thuộc của đối tượng mà đoạn mã của bạn sẽ làm việc cùng: `Factory Method` sẽ `phân tách đoạn mã` khởi tạo sản phẩm và đoạn mã sử dụng sản phẩm. Do đó việc mở rộng mã khởi tạo sản phẩm độc lập với các phần còn lại cũng dễ dàng hơn. 💞

- Sử dụng khi bạn muốn cung cấp cho người dùng  `library` hoặc `framework` với một cách mở rộng thành phần trong nó( ghi đè). 🎁

- Sử dụng khi muốn `tiết kiệm tài nguyên hệ thống` bằng cách sử dụng lại các đối tượng hiện có thay vì xây dựng lại chúng mỗi lần.

# **Proxy Pattern** 🔺🎁🔺
## ☹️ Vấn đề.

Bạn cần hỗ trợ các đối tượng sử dụng nhiều tài nguyên và bạn `không muốn khởi tạo` các đối tượng đó cho đến khi khách hàng yêu cầu chúng.

## 🌞 Giải pháp 

Mẫu `Proxy` gợi ý giải quyết vấn đề trên bằng cách tạo ra một lớp `Proxy` mới có giao diện giống như một đối tượng dịch vụ ban đầu. Sau đó bạn cập nhật ứng dụng của mình để ứng dụng chuyển đối tượng `Proxy` đến tất cả các khách hàng của đối tượng ban đầu. Khi nhận được yêu cầu từ khách hàng, `Proxy` sẽ tạo một đối tượng `dịch vụ thực` và `ủy quyền` tất cả công việc cho nó.

## 🔍 Định nghĩa

<p align="center">
  <img src="https://i.postimg.cc/RVsTBMDW/proxyamh.png">
  <p align="center">Hình 4:  Minh họa cách hoạt động của Proxy Pattern.</p>
</p>

`Proxy` là một mẫu thiết kế thuộc nhóm Structural cung cấp một đại diện thay thế hoặc trình giữ chỗ cho một đối tượng khác để kiểm soát quyền truy cập vào nó. Một `Proxy` kiểm soát quyền truy cập vào đối tượng ban đầu, cho phép bạn thực hiện điều gì đó trước và sau khi được chuyển đến đối đối tượng ban đầu.

## ⚙ Cấu trúc
***Biểu đồ lớp***
<p align="center">
  <img src="https://i.postimg.cc/wj4LS03L/Proxy.png">
  <p align="center">Hình 5: Biểu đồ lớp của Proxy Pattern.</p>
</p>

>Trong đó:

>- `Subject`: là một `giao diện` định nghĩa các phương thức để giao tiếp với client. Đối tượng này xác định giao diện chung cho `RealSubject` và `Proxy` để `Proxy` có thể sử dụng bất cứ nơi nào mà `RealSubject` mong đợi.

>-  `Proxy`: là một `class` sẽ thực hiện các bước kiểm tra và gọi tới đối tượng của class `RealSubject` để thực hiện các thao tác sau khi kiểm tra xong. Nó duy trì một tham chiếu cho phép Proxy truy cập vào chủ thực thể. Nó thực hiện các giao diện tương tự như `RealSubject`.

>- `RealSubject`: là một `class service` sẽ thực hiện các thao tác `thực sự`.

 ***Biểu đồ trình tự***

<p align="center">
  <img src="https://i.postimg.cc/VkFm6VX0/Seque-Proxy.png">
  <p align="center">Hình 6: Biểu đồ trình tự của Proxy Pattern.</p>
</p>

## 📚 Code minh họa

Trong khi một website hiển thị ảnh, có nhiều ảnh trên một trang hay có ảnh được hiển thị nhiều lần. Trường hợp này chúng ta chỉ cần load ảnh khi nó cần hiển thị hoặc là chưa được load, hoàn toan khác so với truyền thống khi tải trang là phải tải hết ảnh xuống.
```
public interface Image {
	void showImage();
}

public class RealImage implements Image {
	private String url;
	
	public RealImage(String url) {
		this.url = url;
		System.out.println("Image loaded: " + this.url);
	}

	@Override
	public void showImage() {
		System.out.println("Image showed: " + this.url);
	}
}

public class ProxyImage implements Image {
	private Image realImage;
	private String url;
	
	public ProxyImage(String url) {
		this.url = url;
		System.out.println("Image unloaded: " + this.url);
	}
	
	@Override
	public void showImage() {
		if (realImage == null) {
			realImage = new RealImage(this.url);
		} else {
			System.out.println("Image already existed: " + this.url);
		}
		realImage.showImage();
	}
}

public class Client {
	public static void main(String[] args) {
		System.out.println("Init proxy image: ");
		ProxyImage proxyImage = ProxyImage("https://gpcoder.com/favicon.ico");
		System.out.println("---");
		System.out.println("Call real service 1st: ");
		proxyImage.showImage();
		System.out.println("---");
		System.out.println("Call real service 2nd: ");
		proxyImage.showImage();
	}
}
```
## ✅ Khả năng áp dụng

- Khi muốn `bảo vệ` `quyền truy xuất` vào các phương thức của object thực.🤗
- Khi cần một số `thao tác` bổ sung trước khi thực hiện các phương thức của `object` thực.
- Khi có `nhiều` truy nhập vào đối tượng có `chi phí` khởi tạo `lớn`.🤗👍
- Khi hệ thống yêu cầu sự chậm trễ khi tải một số tài nguyên nhất định( chỉ tải khi cần thiết).
- Khi muốn `theo dõi` trạng thái và vòng đời đối tượng.
***
## Nguồn 📡

>  "Báo cáo bài tập lớn Project 2: **"Tìm hiểu 10 mẫu thiết kế hướng đối tượng" ".**

## Tài liệu tham khảo ✍️

>1. [Mẫu thiết kế  Factory Method](https://refactoring.guru/design-patterns/factory-method)
>2. [Design Patterns: Elements of Reusable Object-Oriented Software- Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides](https://bom.to/cUg7HHRxd1bp0)

