Python
import json
import os

class ResumeGenerator:
    def __init__(self, data_file):
        self.data_file = data_file
        self.resume_data = {}

    def load_data(self):
        """加载 JSON 格式的简历数据"""
        if not os.path.exists(self.data_file):
            raise FileNotFoundError(f"找不到数据文件: {self.data_file}")
        with open(self.data_file, 'r', encoding='utf-8') as f:
            self.resume_data = json.load(f)

    def generate_markdown(self, output_file="README.md"):
        """将数据转换为精面的 Markdown 格式并写入文件"""
        d = self.resume_data
        
        md_content = f"# 📄 {d.get('name', '个人简历')}\n\n"
        
        # 基本信息 (使用一行简洁展示)
        info = [
            f"**💼 意向岗位:** {d.get('target_job', 'N/A')}",
            f"**📧 邮箱:** {d.get('email', 'N/A')}",
            f"**🌐 博客/GitHub:** [{d.get('website_name', '链接')}]({d.get('website_url', '#')})"
        ]
        md_content += " | ".join(info) + "\n\n---\n\n"
        
        # 个人简介
        md_content += f"## 👤 个人简介\n>{d.get('summary', '')}\n\n"
        
        # 核心技能 (使用 Markdown 标签样式)
        md_content += "## 🛠 核心技能\n"
        for skill in d.get('skills', []):
            md_content += f"`{skill}` "
        md_content += "\n\n"
        
        # 工作/项目经历
        md_content += "## 🚀 项目与工作经历\n"
        for exp in d.get('experience', []):
            md_content += f"### {exp.get('title')} ({exp.get('time')})\n"
            md_content += f"**职责/角色:** {exp.get('role')}\n\n"
            md_content += "**主要贡献:**\n"
            for detail in exp.get('details', []):
                md_content += f"- {detail}\n"
            md_content += "\n"
            
        # 写入文件
        with open(output_file, 'w', encoding='utf-8') as f:
            f.write(md_content)
        print(f"✨ 简历已成功生成至: {output_file}")

if __name__ == "__main__":
    # 模拟一个简单的简历数据，实际使用时可以从本地 resume.json 读取
    sample_data = {
        "name": "张三 (Alex)",
        "target_job": "Python 高级开发工程师",
        "email": "alex@example.com",
        "website_name": "GitHub 主页",
        "website_url": "https://github.com",
        "summary": "5年 Python 开发经验，热衷于自动化工具开发与高并发后端架构设计。极客精神，代码强迫症患者。",
        "skills": ["Python", "Django", "FastAPI", "Docker", "Redis", "MySQL", "Markdown"],
        "experience": [
            {
                "title": "某大型互联网公司 - 自动化效能组",
                "time": "2023.06 - 至今",
                "role": "后端核心开发",
                "details": [
                    "主导重构了内部 CI/CD 自动化流水线，将部署效率提升了 40%。",
                    "基于 FastAPI 开发了微服务监控看板，支撑每日千万级数据打点。"
                ]
            }
        ]
    }
    
    # 将模拟数据写入临时的 json
    with open("resume.json", "w", encoding="utf-8") as f:
        json.dump(sample_data, f, ensure_ascii=False, indent=4)
        
    # 运行生成器
    generator = ResumeGenerator("resume.json")
    generator.load_data()
    generator.generate_markdown()
