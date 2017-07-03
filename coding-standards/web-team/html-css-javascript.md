### [Front-end Deveplopment practice](#)
***
1. **Không bao giờ để lẫn CSS, HTML, Javascript. CSS file riêng, Javascript file riêng tách biệt với HTML code**
2. CSS
	+ Nên đặt tên **id**, **class** dưới dạng giống như Bootstrap (có dấu - giữa các từ). Ví dụ: 
				
			<div class=""col-lg-offset-1" id="user-login-div"></div>
			
	+ Còn **name** của các element thì nên theo naming của framework serverside đang sử dụng (với Laravel 4 thì là camel case). Ví dụ với Laravel 4: 
	
			<input name="userLanguage" class="input-long"></input>
			
3. Nên sử dụng 1 front-end framework (Bootstrap,…)
4. Optimize các ảnh trước khi sử dụng cho web (png hoặc gif)
5. Code HTML cần rõ cấu trúc, phần nào ra phần đấy, không lẫn lỗn các thẻ
6. Khi đóng 1 thẻ div cần comment đã đóng thẻ nào

		<div id="header">
  			<div id="sub" class="first left">
    			...
  			</div><!-- #sub.first.left -->
		</div><!-- #header -->
		
7. Import file javascript ở cuối file html

		 		...
    			<script type='text/javascript' src='jquery.js?ver=1.8.1'></script>
  			</body>
		</html>
		
8. Import file css trong thẻ head ở đầu file

			<head>
				...
				<link rel='stylesheet' type='text/css' href='a.css'>
			</head>
		
9. To be continued…  
  

### [Coding Standards](#)
***
1. **Đối với CSS, HTML, Javascript, cài đặt "tab space"(tabstop) cho editor là 2**
2. To be continued..
