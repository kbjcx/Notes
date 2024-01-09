# Remove Space In String
去除字符串中的多余空格，包括首尾空格与字符串中的重复空格
```cpp
std::string trim(const std::string& str) {
    size_t start = str.find_first_not_of(' ');
    size_t end = str.find_last_not_of(' ');

    if (start == std::string::npos || end == std::string::npos) {
        return "";
    }

    return str.substr(start, end - start + 1);
}

std::string removeExtraSpaces(std::string& str) {
    int left = 0, right = 0;
    str = trim(str);
    for (; right < str.size(); ++right) {
        if (right > 0 && str[right] == ' ' && str[right - 1] == ' ') {
            continue;
        } else {
            str[left] = str[right];
            ++left;
        }
    }

    str.resize(left);

    return str;
}
```