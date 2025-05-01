<!DOCTYPE html>
<html lang="zh-CN">
<head>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    <meta charset="UTF-8">
    <title>跨时空邮局 - 致简爱的信</title>
    <!-- 新增 Supabase 客户端库 -->
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    <style>
        /* 原有样式保持不变 */
    </style>
</head>
<body>
    <!-- 原有 HTML 结构保持不变 -->

    <script>
        // 初始化 Supabase 客户端
        const supabase = createClient(
            "https://buzcvaovnqufsxgipfyk.supabase.co
",
            "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImJ1emN2YW92bnF1ZnN4Z2lwZnlrIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDYwNjAwODQsImV4cCI6MjA2MTYzNjA4NH0._gealYEfxKJjwI0QkpIHFAEd9imnfwajIah_tuVf2Rc
"
        );

        const input = document.getElementById('letter-input');
        const lettersContainer = document.getElementById('letters-container');

        // 页面加载时获取历史信件
        window.addEventListener('DOMContentLoaded', async () => {
            await loadLetters();
        });

        // 提交信件
        document.getElementById('submit-btn').addEventListener('click', async function() {
            const content = input.value.trim();
            if (!content) return;

            try {
                // 插入数据库
                const { data, error } = await supabase
                    .from('letters')
                    .insert([{ 
                        content: content,
                        created_at: new Date().toISOString()
                    }]);

                if (error) throw error;
                
                // 添加新信件到列表顶部
                addLetterToUI(content, new Date().toLocaleString());
                input.value = '';
                
                // 按钮反馈
                this.textContent = '信件已寄出 ✨';
                setTimeout(() => this.textContent = '寄出这封信', 2000);
            } catch (error) {
                console.error('保存失败:', error);
                alert('寄信失败，请稍后再试');
            }
        });

        // 加载历史信件
        async function loadLetters() {
            try {
                const { data, error } = await supabase
                    .from('letters')
                    .select('*')
                    .order('created_at', { ascending: false });

                if (error) throw error;
                
                data.forEach(letter => {
                    addLetterToUI(
                        letter.content,
                        new Date(letter.created_at).toLocaleString()
                    );
                });
            } catch (error) {
                console.error('加载失败:', error);
            }
        }

        // 添加信件到界面
        function addLetterToUI(content, time) {
            const letterDiv = document.createElement('div');
            letterDiv.className = 'letter-item';
            letterDiv.innerHTML = `
                <div class="letter-time">⏳ ${time}</div>
                <div class="letter-content">${content.replace(/\n/g, '<br>')}</div>
            `;
            lettersContainer.prepend(letterDiv);
        }
    </script>
</body>
</html># johnsmith.github.io
