---
title: "Blog 1"
date: "2025-07-09"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Cách Patronus AI Giúp Doanh Nghiệp Nâng Cao Độ Tin Cậy Khi Sử Dụng Generative AI

bởi Aditya Shahani và Bonnie McClure | vào NGÀY 02 THÁNG 5, 2024 | trong [Giải pháp Khách hàng](https://aws.amazon.com/vi/blogs/startups/category/customer-solutions/), [Generative AI](https://aws.amazon.com/vi/blogs/startups/category/generative-ai/), [Startup](https://aws.amazon.com/vi/blogs/startups/category/startup/)

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-1/patronuslogo.png"
         alt="Patronus AI Logo"
         width="260"
         style="display:block; margin-left:0;">
</div>

Trong những năm gần đây, đặc biệt kể từ khi **ChatGPT** ra mắt năm 2022, tiềm năng mang tính thay đổi của **Generative AI** đã trở nên không thể phủ nhận đối với các tổ chức thuộc mọi quy mô và trong nhiều ngành nghề. Làn sóng tiếp theo của quá trình ứng dụng đã bắt đầu, khi các doanh nghiệp đang nhanh chóng triển khai các công cụ Generative AI để tăng hiệu quả và cải thiện trải nghiệm khách hàng. Theo [Báo cáo McKinsey 2023](https://www.mckinsey.com/capabilities/mckinsey-digital/our-insights/the-economic-potential-of-generative-ai-the-next-productivity-frontier), Generative AI có thể đóng góp thêm từ 2,6 đến 4,4 nghìn tỷ USD cho nền kinh tế toàn cầu mỗi năm, làm tăng tác động kinh tế tổng thể của AI khoảng 15–40%. Bên cạnh đó, khảo sát CEO toàn cầu mới nhất của **IBM** cho thấy 50% người tham gia đã bắt đầu tích hợp Generative AI vào sản phẩm và dịch vụ của họ.

<div style="float: right; margin: 0 0 1.5em 1.5em; text-align: center; width: 220px;">
    <img src="/images/3-BlogsTranslated/Blog-1/Anand_300x361.png" alt="Anand Kannappan Co-founder & CEO | Patronus AI" style="width: 361px; height: 300px; object-fit: cover; display: block; margin: 0 auto;">
</div>

Tuy nhiên, khi Generative AI trở nên phổ biến, khách hàng và doanh nghiệp ngày càng lo ngại về tính tin cậy của công nghệ này. Ngoài ra, cũng thường không rõ lý do vì sao đầu vào cụ thể lại tạo ra những đầu ra nhất định, khiến các công ty khó đánh giá một cách chính xác kết quả mà Generative AI tạo ra. [Patronus AI](http://patronus.ai/), do các chuyên gia học máy (ML) Anand Kannappan và Rebecca Qian sáng lập, đã đặt mục tiêu giải quyết vấn đề này. Với nền tảng đánh giá và bảo mật tự động dựa trên AI, Patronus giúp khách hàng sử dụng các mô hình ngôn ngữ lớn (LLM) một cách tự tin và có trách nhiệm, đồng thời giảm thiểu rủi ro sai sót. Mục tiêu của startup là nâng cao tính đáng tin cậy và khả năng sử dụng của các mô hình AI. Anand cho biết: “Đó là câu hỏi lớn nhất trong năm qua. Mọi doanh nghiệp đều muốn sử dụng mô hình ngôn ngữ, nhưng họ lo ngại về rủi ro và cả độ tin cậy của chúng, đặc biệt trong các tình huống đặc thù của doanh nghiệp.” Anh nói thêm: “Sứ mệnh của chúng tôi là tăng cường sự tự tin của doanh nghiệp khi ứng dụng Generative AI.”

---

## Tận dụng lợi ích và quản lý rủi ro khi dùng Generative AI

Generative AI là một dạng AI sử dụng ML để tạo ra dữ liệu mới giống với dữ liệu đã được dùng để huấn luyện. Bằng cách học các mẫu và cấu trúc của dữ liệu đầu vào, nó có thể tạo ra nội dung nguyên bản — như hình ảnh, văn bản, hay thậm chí mã lập trình. Các ứng dụng Generative AI được vận hành bởi các mô hình đã được huấn luyện trước trên lượng dữ liệu khổng lồ, đặc biệt là các LLM được huấn luyện trên hàng nghìn tỷ từ.

Lợi ích kinh doanh mà Generative AI mang lại là vô cùng lớn. Nhiều công ty đang quan tâm đến việc sử dụng LLM để khai thác dữ liệu nội bộ thông qua tìm kiếm, tạo memo hoặc bài thuyết trình, cải thiện chatbot tự động, hoặc hỗ trợ viết mã. Anand cũng cho biết còn rất nhiều ngành chưa được "chạm tới" bởi Generative AI. “Chúng ta mới chỉ ở giai đoạn đầu của những gì có thể đạt được.”

<div style="float: right; margin: 0 0 1.5em 1.5em; text-align: center; width: 220px;">
    <img src="/images/3-BlogsTranslated/Blog-1/Rebecca_300x361.png" alt="Anand Kannappan Co-founder & CEO | Patronus AI" style="width: 400px; height: 300px; object-fit: cover;  display: block; margin: 0 auto;">
</div>

Khi doanh nghiệp mở rộng ứng dụng Generative AI, vấn đề tin cậy trở nên cấp thiết hơn. Người dùng muốn đảm bảo rằng đầu ra của mô hình tuân thủ quy định và chính sách nội bộ, đồng thời không dẫn đến các kết quả sai lệch hoặc nguy hiểm. Anand chia sẻ: “Đối với các công ty lớn, đặc biệt trong những ngành bị quản lý nghiêm ngặt, có nhiều tình huống mang tính sống còn. Họ muốn sử dụng Generative AI nhưng lo rằng chỉ một sai sót cũng có thể làm tổn hại đến thương hiệu hoặc gây rủi ro cho khách hàng.”

Patronus giúp khách hàng quản lý rủi ro bằng cách cải thiện khả năng đo lường và phân tích hiệu năng mô hình. “Điều quan trọng là phải đảm bảo quy trình kiểm thử và đánh giá phải thật sự vững chắc và chuẩn hóa,” Anand nói. “Hiện tại vẫn chưa có một khuôn khổ tiêu chuẩn nào để kiểm thử mô hình ngôn ngữ một cách khoa học.”

---

## Nâng cao độ tin cậy và hiệu suất

Nền tảng tự động của Patronus giúp khách hàng đánh giá và so sánh hiệu suất của nhiều mô hình LLM trong các tình huống thực tế, giảm nguy cơ sinh ra đầu ra không mong muốn. Patronus sử dụng các kỹ thuật ML mới để tự động tạo bộ kiểm thử đối kháng và chấm điểm mô hình dựa trên hệ thống tiêu chí độc quyền. Ví dụ, bộ dữ liệu FinanceBench là bộ benchmark đầu tiên đo hiệu suất LLM trên các câu hỏi tài chính.

“Mọi thứ chúng tôi làm đều hướng tới việc giúp các công ty phát hiện lỗi mô hình ở quy mô lớn và hoàn toàn tự động,” Anand nói. Hiện nay, nhiều doanh nghiệp đang phải chi những khoản rất lớn cho đội ngũ QA nội bộ và tư vấn bên ngoài — những người phải tạo test case thủ công và chấm điểm bằng bảng tính. Cách tiếp cận của Patronus giúp loại bỏ quy trình tốn kém đó.

Anand giải thích: “Xử lý ngôn ngữ tự nhiên là lĩnh vực rất thực nghiệm, nên chúng tôi phải thử nghiệm liên tục để tìm ra kỹ thuật đánh giá tối ưu nhất.” Anh nhấn mạnh mục tiêu là giúp doanh nghiệp dễ dàng tận dụng các kỹ thuật này để cải thiện hiệu suất — cả trong mô hình của họ lẫn quá trình đánh giá mô hình.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-1/Enhancing1.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

Một vòng tròn cải thiện liên tục được tạo ra: doanh nghiệp càng sử dụng nhiều và phản hồi nhiều, các đánh giá càng chính xác hơn, và hệ thống nội bộ của doanh nghiệp cũng cải thiện theo.

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-1/Enhancing2.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

## Tăng cường sự tự tin thông qua kết quả tốt hơn và khả năng hiểu mô hình

Để khai thác hết tiềm năng của Generative AI, việc nâng cao độ tin cậy và tính giải thích của mô hình là cực kỳ quan trọng. Nhiều doanh nghiệp đang gặp khó khăn không chỉ vì mô hình đôi khi sai mà còn vì không hiểu được tại sao sai, và làm sao để tránh lặp lại.

Anand nói: “Điều mọi người muốn là một cách để tự tin hơn khi đưa hệ thống vào sản xuất. Khi bạn cho nhân viên hay khách hàng sử dụng, đó có thể là hàng nghìn người. Bạn muốn hạn chế tối đa rủi ro. Và khi lỗi xảy ra, bạn muốn biết nguyên nhân.”

Một trong những mục tiêu lớn của Patronus là tăng khả năng giải thích — hiểu được tại sao mô hình đưa ra kết quả như vậy và làm thế nào để cải thiện. Patronus cung cấp lời giải thích bằng ngôn ngữ tự nhiên cho từng test case, giúp khách hàng nhanh chóng hiểu nguyên nhân thất bại và nhận các gợi ý cải thiện prompt hoặc tham số mô hình.

---

## Hướng đến tương lai của Generative AI cùng AWS

Ngay từ đầu, Patronus đã xây dựng ứng dụng của mình trên AWS. Công ty sử dụng nhiều dịch vụ cloud như [Amazon SQS](https://aws.amazon.com/sqs/) cho hạ tầng hàng đợi và [Amazon EC2](https://aws.amazon.com/ec2/) cho môi trường Kubernetes, đồng thời tận dụng sự tùy chỉnh linh hoạt của [Amazon EKS](https://aws.amazon.com/eks/).

Nhờ kinh nghiệm lâu năm làm việc với AWS trước khi sáng lập Patronus, Anand và nhóm của ông có thể nhanh chóng phát triển sản phẩm và hạ tầng. Patronus cũng hợp tác chặt chẽ với đội ngũ AWS chuyên hỗ trợ startup. Anand chia sẻ: “Tư duy lấy khách hàng làm trung tâm của AWS luôn tuyệt vời, và chúng tôi rất trân trọng điều đó.”

Patronus hiện đang lạc quan hướng về tương lai, sau khi ra mắt và thu hút được sự quan tâm lớn cùng nguồn vốn hạt giống 3 triệu USD từ [Lightspeed Venture Partners](https://lsvp.com/). Nhóm cũng đã công bố benchmark đầu tiên về hiệu suất LLM trong các câu hỏi tài chính — hợp tác thiết kế cùng 15 chuyên gia trong ngành.

Anand nói: “Chúng tôi rất hào hứng với những gì sắp tới. Chúng tôi sẽ tiếp tục tập trung vào đánh giá và kiểm thử AI, giúp doanh nghiệp xác định điểm yếu trong mô hình, đo lường hiệu suất và cuối cùng là xây dựng những sản phẩm đáng tin cậy hơn.”

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/3-BlogsTranslated/Blog-1/anand-and-rebecca.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

---

## Về các tác giả

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-1/Aditya-Shahani.jpg" alt="Aditya Shahani" style="width: 100px; height: 100px; object-fit: cover; border-radius: 50%; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;"> 
        <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Aditya Shahani</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Aditya Shahani là Kiến trúc sư Giải pháp dành cho Startup, tập trung hỗ trợ các startup giai đoạn đầu tăng tốc hành trình xây dựng trên AWS. Anh đam mê sử dụng công nghệ mới để giải quyết các vấn đề kinh doanh ở quy mô lớn.
        </p>
    </div>
</div>

<div style="display: flex; align-items: center; border: 1px solid #e1e4e8; padding: 20px; margin-bottom: 20px; background-color: #fff; text-align: left;">
    <img src="/images/3-BlogsTranslated/Blog-1/Bonnie-headshot-SQ.jpg" alt="Bonnie McClure" style="width: 100px; height: 100px; object-fit: cover; border-radius: 50%; margin: 0 20px 0 0 !important; flex-shrink: 0; display: block;">
    <div style="flex: 1;">
        <h3 style="margin: 0 0 5px 0; font-size: 18px; color: #232f3e; font-weight: bold; font-family: sans-serif;">Bonnie McClure</h3>
        <p style="margin: 0; color: #555; line-height: 1.5; font-size: 14px; font-family: sans-serif;">
            Bonnie là biên tập viên chuyên tạo nội dung dễ tiếp cận và thu hút cho mọi đối tượng và nền tảng. Cô cam kết mang đến trải nghiệm người dùng mượt mà thông qua quy trình biên tập toàn diện.
        </p>
    </div>
</div>
