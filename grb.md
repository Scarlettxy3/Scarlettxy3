from PyPDF2 import PdfReader
import re

# 文件路径
file_path = '/mnt/data/Amina Trabelsi, Saad Ouichaoui, et al..pdf'

def extract_references(pdf_path):
    """从PDF文档中提取参考文献"""
    reader = PdfReader(pdf_path)
    references = []
    
    # 遍历所有页面
    for page in reader.pages:
        text = page.extract_text()
        # 假设参考文献以"References"开始
        if "References" in text:
            # 提取"References"部分后内容
            ref_text = text.split("References", 1)[1]
            # 按行分割，并筛选可能的文献格式
            ref_lines = ref_text.split("\n")
            for line in ref_lines:
                # 简单正则匹配文献格式
                if re.match(r"\d+\.\s[A-Z][a-z]+", line):
                    references.append(line.strip())
            break  # 停止在找到“References”部分后

    return references

# 提取参考文献
references = extract_references(file_path)

# 输出参考文献
if references:
    print("提取到的参考文献：")
    for ref in references:
        print(ref)
else:
    print("未找到参考文献部分。")
