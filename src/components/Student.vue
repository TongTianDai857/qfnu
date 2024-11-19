<template>
  <div class="container">
    <div class="sidebar">
      <div class="nav-item" 
        v-for="(item, index) in modules" 
        :key="index"
        @click="currentModule = item">
        {{ item.name }}
      </div>
    </div>
      <div class="content">
        <div class="module" v-if="currentModule">
          <div v-if="currentModule.name === '总览'" class="module">
            <div v-for="module in modules" :key="module.name" class="overview-section">
              <h3 class="overview-title">{{ module.name }}</h3>
              <div v-if="module.content" class="overview-content">
                <div v-if="module.name === '总览'" id="overview">
                  <div class="NewNotice">
                    <h3>最新公告</h3>
                    <h4 style="text-align: center;">{{ notice.title[notice.title.length-1] }}</h4>
                    <p style="text-align: center;">时间：{{ formatDate(notice.time[notice.time.length-1]) }} 发起人：{{ notice.user[notice.user.length-1] }}</p>
                    <p v-for="line in Lines(notice.content.length-1)">{{ line }}</p>
                  </div>
                  <div class="Information">
                    <h3>个人信息</h3>
                    姓名:{{ studentName }}<br>
                    学号:{{ studentId }}<br>
                    班级:{{ studentClass }}
                  </div>
                  <div class="Information">
                    <h3>活动任务</h3>
                    剩余:{{ remainRequestNum }}
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div v-else-if="currentModule.name === '公告'">
            <div class="Notice" v-for="(item, index) in notice.title":key="index">
              <h3 style="text-align: center;">{{ notice.title[notice.title.length-index-1] }}</h3>
              <p style="text-align: center;">时间：{{ formatDate(notice.time[notice.time.length-index-1]) }} 发起人：{{ notice.user[notice.user.length-index-1] }}</p>
              <p v-for="line in Lines(notice.title.length-index-1)">{{ line }}</p>
            </div>
          </div>
          <div v-else-if="currentModule.name === '活动任务'">
            test
          </div>
          <div v-else>
            <h2>{{ currentModule.name }}</h2>
            <textarea v-model="currentModule.content" 
                      placeholder="请在这里输入内容..."></textarea>
            <button @click="saveContent">保存</button>
        </div>
      </div>
    </div>
  </div>
</template>
  
<script>
  import dayjs from 'dayjs';
  export default {
    data() {
      return {
        currentModule: null,
        modules: [
          {
            name: '总览',
            content: '1',
            history: []
          },
          {
            name: '公告',
            content: ''
          },
          {
            name: '活动任务',
            content: '',
            history: []
          },
          {
            name: '个人计划',
            content: '',
            history: []
          },
          {
            name: '个人奖项',
            content: '',
            history: []
          },
          {
            name: '心得体会',
            content: '',
            history: []
          }
        ]
      };
    },
    computed: {
      sortedHistory() {
        return this.currentModule.history
          ? [...this.currentModule.history].sort((a, b) => b.timestamp - a.timestamp)
          : [];
      }
    },
    mounted() {
      // 默认显示总览模块
      this.currentModule = this.modules[0];
      // 从服务器加载数据
      this.loadContent();
      this.getStudentData();
    },
    methods: {
      formatDate(timestamp) {
        return new Date(timestamp).toLocaleString();
      },
      async saveContent() {
        try {
          if (!this.currentModule.history) {
            this.currentModule.history = [];
          }
  
          this.currentModule.history.push({
            content: this.currentModule.content,
            timestamp: Date.now()
          });
  
          // 发送数据到服务器
          const response = await fetch('/api/student/modules', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
              'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
            body: JSON.stringify(this.modules)
          });
  
          if (!response.ok) {
            throw new Error('保存失败');
          }
  
          alert('保存成功！');
        } catch (error) {
          alert('保存失败：' + error.message);
          console.error('保存失败：', error);
        }
      },
      async loadContent() {
        /*try {
          // 从服务器获取数据
          const response = await fetch('/api/student/modules', {
            method: 'GET',
            headers: {
              'Authorization': 'Bearer ' + localStorage.getItem('token')
            }
          });
  
          if (!response.ok) {
            throw new Error('加载失败');
          }
  
          const data = await response.json();
          this.modules = data;
        } catch (error) {
          alert('加载失败：' + error.message);
          console.error('加载失败：', error);
        }*/
      },
      getStudentData() {
        console.log()
        this.studentName = '姓名'
        this.studentId = '学号'
        this.studentClass = '班级'
        this.remainRequestNum = 1
        this.notice = {
          title: ['公告1','公告2','公告3'],
          content: ['这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条','测试','滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试'],
          time: ['2024-09-01T00:00:00+08:00','2024-10-01T00:00:00+08:00','2024-11-01T00:00:00+08:00'],
          user: ['admin1','admin2','admin3']
        }
      },
      Lines(i) {
        return this.notice.content[i].split('\n')
      },
      formatDate(date) {
        return dayjs(date).format('YYYY年MM月DD日 HH:mm:ss')
      }
    }
  };
  </script>
  
  <style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  .container {
    display: flex;
    min-height: 100vh;
  }
  
  .sidebar {
    width: 200px;
    background-color: #2c3e50;
    padding: 20px;
    color: white;
  }
  
  .nav-item {
    padding: 10px;
    margin: 5px 0;
    cursor: pointer;
    border-radius: 5px;
    transition: background-color 0.3s;
  }
  
  .nav-item:hover {
    background-color: #34495e;
  }
  
  .content {
    flex: 1;
    padding: 20px;
  }
  
  .module {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  
  textarea {
    width: 100%;
    min-height: 200px;
    margin: 10px 0;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  
  button {
    background-color: #3498db;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #2980b9;
  }
  
  .history-list {
    margin-top: 20px;
    border-top: 1px solid #ddd;
    padding-top: 20px;
  }
  
  .history-item {
    padding: 10px;
    border: 1px solid #eee;
    margin-bottom: 10px;
    border-radius: 4px;
  }
  
  .history-meta {
    font-size: 0.9em;
    color: #666;
    margin-bottom: 5px;
  }
  
  .overview-section {
    margin-bottom: 30px;
  }
  
  .overview-title {
    font-size: 1.2em;
    color: #2c3e50;
    margin-bottom: 10px;
    padding-bottom: 5px;
    border-bottom: 2px solid #3498db;
  }
  
  .overview-content {
    background-color: #f9f9f9;
    padding: 15px;
    border-radius: 4px;
    white-space: pre-wrap;
  }
  
  .empty-message {
    color: #666;
    font-style: italic;
  }
  
  .Information {
    width: 200px;
    height: 200px;
    box-shadow: 0px 0px 0px #ccc;
    border-radius: 10px;
    border: 3px solid #00afee;
    padding: 10px;
    line-height: 2;
    margin: 5px;
  }

  #overview {
    display: flex;
    flex-wrap: wrap;
  }

  .NewNotice {
    width: 100%;
    height: 400px;
    box-shadow: 0px 0px 0px #ccc;
    border-radius: 10px;
    border: 3px solid #00afee;
    padding: 10px;
    line-height: 2;
    margin: 5px;
    white-space: wrap;
    overflow: auto;
  }
  .Notice {
    width: 100%;
    box-shadow: 0px 0px 0px #ccc;
    border-radius: 10px;
    border: 3px solid #00afee;
    padding: 10px;
    line-height: 2;
    margin: 5px;
    white-space: wrap;
    overflow: auto;
  }

  </style>
  