<template>
    <div class="friends-list">
      <h2>好友列表</h2>
      
      <!-- 搜索好友 -->
      <input
        v-model="searchQuery"
        placeholder="搜索好友..."
        @input="searchFriends"
      />
      
      <!-- 添加好友 -->
      <div class="add-friend">
        <input
          v-model="newFriend"
          placeholder="输入好友用户名..."
        />
        <button @click="addFriend">添加好友</button>
      </div>
  
      <!-- 好友分组 -->
      <div class="friend-groups">
        <label for="groupSelect">分组</label>
        <select id="groupSelect" v-model="selectedGroup" @change="filterByGroup">
          <option value="">所有好友</option>
          <option v-for="group in groups" :key="group">{{ group }}</option>
        </select>
      </div>
  
      <!-- 好友列表 -->
      <ul>
        <li v-for="friend in filteredFriends" :key="friend.id">
          <span>{{ friend.name }}</span>
          <span v-if="friend.online" class="online-status">(在线)</span>
          <span v-else class="offline-status">(离线)</span>
          <button @click="sendMessage(friend)">私信</button>
          <button @click="blockFriend(friend)">拉黑</button>
          <button @click="removeFriend(friend)">删除好友</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import { ref, computed } from 'vue';
  
  export default {
    name: 'FriendsList',
    setup() {
      // 搜索相关
      const searchQuery = ref('');
      const newFriend = ref('');
      const friends = ref([
        { id: 1, name: 'Alice', online: true, group: '家人' },
        { id: 2, name: 'Bob', online: false, group: '同事' },
        { id: 3, name: 'Charlie', online: true, group: '仅好友' },
        // 其他好友
      ]);
      
      // 分组相关
      const groups = ref(['家人', '同事', '仅好友']);
      const selectedGroup = ref('');
  
      // 计算过滤后的好友列表
      const filteredFriends = computed(() => {
        let filtered = friends.value;
        // 按分组筛选
        if (selectedGroup.value) {
          filtered = filtered.filter(friend => friend.group === selectedGroup.value);
        }
        // 按搜索关键词筛选
        if (searchQuery.value) {
          filtered = filtered.filter(friend => 
            friend.name.toLowerCase().includes(searchQuery.value.toLowerCase())
          );
        }
        return filtered;
      });
  
      // 搜索好友
      const searchFriends = () => {
        // 搜索逻辑
      };
  
      // 添加好友
      const addFriend = () => {
        if (newFriend.value) {
          friends.value.push({
            id: Date.now(),
            name: newFriend.value,
            online: false,
            group: '未分组', // 默认分组
          });
          newFriend.value = ''; // 清空输入框
        }
      };
  
      // 删除好友
      const removeFriend = (friend) => {
        friends.value = friends.value.filter(f => f.id !== friend.id);
      };
  
      // 私信好友
      const sendMessage = (friend) => {
        alert(`正在与${friend.name}聊天`);
      };
  
      // 拉黑好友
      const blockFriend = (friend) => {
        if (confirm(`确定要拉黑${friend.name}吗？`)) {
          // 拉黑逻辑
        }
      };
  
      // 分组筛选
      const filterByGroup = () => {
        // 分组筛选逻辑
      };
  
      return {
        searchQuery,
        newFriend,
        friends,
        groups,
        selectedGroup,
        filteredFriends,
        searchFriends,
        addFriend,
        removeFriend,
        sendMessage,
        blockFriend,
        filterByGroup,
      };
    }
  };
  </script>
  
  <style scoped>
  .friends-list {
    max-width: 400px;
    margin: 0 auto;
  }
  
  input {
    display: block;
    width: 100%;
    margin-bottom: 10px;
    padding: 8px;
  }
  
  ul {
    list-style: none;
    padding: 0;
  }
  
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid #ccc;
  }
  
  .online-status {
    color: green;
  }
  
  .offline-status {
    color: red;
  }
  
  button {
    margin-left: 10px;
    padding: 5px;
  }
  
  .add-friend {
    display: flex;
    justify-content: space-between;
  }
  
  .friend-groups {
    margin-bottom: 15px;
  }
  </style>
