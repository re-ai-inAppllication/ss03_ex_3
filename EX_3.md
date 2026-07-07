Bài tập 3: Đọc hiểu & Dò lỗi qua Prompt - Phát hiện Ảo giác AI
1. Phân tích nguyên lý sinh lỗi ảo giác (Hallucination)
Việc "mớm mồi" một phương thức không có thật như StringUtils.hasSpecialCharacters() vào prompt khiến AI dễ bị ảo giác là do bản chất của các mô hình ngôn ngữ lớn (LLM). Chúng hoạt động dựa trên cơ chế dự đoán từ hoặc cụm từ tiếp theo để tạo ra câu trả lời có xác suất thống kê cao nhất, dựa trên ngữ cảnh được cung cấp. Do được thiết kế để "chiều lòng" người dùng và duy trì tính liền mạch của đoạn hội thoại, AI có xu hướng chấp nhận giả định sai lệch trong câu hỏi (rằng phương thức đó thực sự tồn tại) thay vì từ chối hay kiểm chứng lại. Hậu quả là AI tự động "bịa" ra (hallucinate) cách triển khai cho hàm hoặc lớp đối tượng không có thực đó nhằm hoàn thành yêu cầu một cách logic nhất theo ngữ cảnh giả tưởng.

2. Prompt tối ưu mới
Prompt gửi cho AI:

Viết một phương thức bằng ngôn ngữ Java để kiểm tra xem một chuỗi có chứa ký tự đặc biệt hay không. Yêu cầu bắt buộc:

Chỉ sử dụng các API chính thức, đã được kiểm chứng của thư viện chuẩn Java (ví dụ như java.util.regex.Pattern hoặc các phương thức có sẵn của lớp String). Tuyệt đối không tự bịa ra các thư viện hay phương thức không tồn tại.
Phải giải thích rõ cơ sở hoạt động của phương pháp được sử dụng và trích dẫn/nhắc đến tài liệu chính thống (Official Documentation) để chứng minh phương thức hoặc cách tiếp cận đó thực sự tồn tại trong Java.
3. Phản hồi của AI dựa trên Prompt tối ưu
AI Phản hồi:

Để kiểm tra xem một chuỗi có chứa ký tự đặc biệt hay không trong Java, cách tiếp cận chuẩn xác và tối ưu nhất là sử dụng Regular Expression (Biểu thức chính quy) thông qua các lớp java.util.regex.Pattern và java.util.regex.Matcher thuộc thư viện chuẩn của Java, hoặc sử dụng phương thức tiện ích matches() trực tiếp trên đối tượng String.

Giải thích và Cơ sở tài liệu (Official Documentation):

Theo tài liệu API chính thức của Oracle Java (Java SE Documentation), lớp java.util.regex.Pattern được sử dụng để biên dịch một biểu thức chính quy thành một mẫu (pattern), sau đó tạo ra một bộ khớp (Matcher) để thực hiện các thao tác tìm kiếm trên chuỗi đầu vào.
Phương thức String.matches(String regex) cũng được định nghĩa rõ ràng trong Java API (từ phiên bản JDK 1.4). Nó hoạt động bằng cách gọi ngầm Pattern.matches(regex, this).
Biểu thức chính quy sử dụng trong giải pháp này là [^a-zA-Z0-9]. Lớp ký tự [^...] trong Java Regex Regex mang ý nghĩa phủ định. Cụ thể, nó sẽ tìm kiếm bất kỳ ký tự nào KHÔNG nằm trong các khoảng từ a đến z, A đến Z, và 0 đến 9. Nếu bộ khớp tìm thấy ít nhất một ký tự như vậy, điều đó chứng tỏ chuỗi có chứa ký tự đặc biệt.
Mã nguồn Java hoàn chỉnh:

import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class SpecialCharacterChecker {

    /**
     * Kiểm tra xem chuỗi có chứa ký tự đặc biệt hay không.
     * Ký tự đặc biệt được định nghĩa là bất kỳ ký tự nào không phải là chữ cái (a-z, A-Z) hoặc chữ số (0-9).
     *
     * @param input Chuỗi cần kiểm tra
     * @return true nếu chuỗi chứa ký tự đặc biệt, ngược lại trả về false
     */
    public static boolean hasSpecialCharacters(String input) {
        // Xử lý trường hợp chuỗi null hoặc rỗng
        if (input == null || input.isEmpty()) {
            return false;
        }

        // Định nghĩa biểu thức chính quy: tìm bất kỳ ký tự nào KHÔNG phải chữ cái hoặc chữ số
        String regex = "[^a-zA-Z0-9]";
        
        // Cách 1: Sử dụng Pattern và Matcher (Tối ưu hiệu suất khi biên dịch sẵn nếu dùng nhiều lần)
        Pattern pattern = Pattern.compile(regex);
        Matcher matcher = pattern.matcher(input);
        
        // matcher.find() sẽ trả về true nếu tìm thấy ít nhất một phần của chuỗi khớp với regex
        return matcher.find();
        
        /* 
         * Cách 2: Sử dụng String.matches() (Viết gọn hơn nhưng tạo Pattern mới mỗi lần gọi)
         * Lưu ý: String.matches() kiểm tra khớp TOÀN BỘ chuỗi, 
         * nên regex cần được viết lại thành: ".*[^a-zA-Z0-9].*"
         * 
         * return input.matches(".*[^a-zA-Z0-9].*");
         */
    }

    public static void main(String[] args) {
        // Các kịch bản kiểm thử (Test cases)
        String testString1 = "HelloWorld123";
        String testString2 = "Hello World!"; // Chứa khoảng trắng và dấu chấm than
        String testString3 = "Java@2024";     // Chứa ký tự @

        System.out.println("Chuỗi: '" + testString1 + "' có ký tự đặc biệt? " 
                + hasSpecialCharacters(testString1)); // Kết quả mong đợi: false
                
        System.out.println("Chuỗi: '" + testString2 + "' có ký tự đặc biệt? " 
                + hasSpecialCharacters(testString2)); // Kết quả mong đợi: true
                
        System.out.println("Chuỗi: '" + testString3 + "' có ký tự đặc biệt? " 
                + hasSpecialCharacters(testString3)); // Kết quả mong đợi: true
    }
}