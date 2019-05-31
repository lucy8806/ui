<template>
  <a-drawer
    title="角色信息"
    :maskClosable="false"
    width=650
    placement="right"
    :closable="true"
    @close="close"
    :visible="roleInfoVisiable"
    style="height: calc(100% - 55px);overflow: auto;padding-bottom: 53px;">
      <p><a-icon type="crown" />&nbsp;&nbsp;角色名称：{{roleInfoData.role}}</p>
      <p :title="roleInfoData.remark"><a-icon type="book" />&nbsp;&nbsp;角色描述：{{roleInfoData.description}}</p>
      <p><a-icon type="clock-circle" />&nbsp;&nbsp;创建时间：{{roleInfoData.createtime}}</p>
      <p><a-icon type="clock-circle" />&nbsp;&nbsp;修改时间：{{roleInfoData.updatetime? roleInfoData.updatetime : '暂未修改'}}</p>
      <p><a-icon type="trophy" />&nbsp;&nbsp;所拥有的权限：
        <a-tree
          :key="key"
          :checkStrictly="true"
          :checkable="true"
          :defaultCheckedKeys="checkedKeys[0]"
          :defaultExpandedKeys="checkedKeys[0]"
          :treeData="menuTreeData">
        </a-tree>
      </p>
  </a-drawer>
</template>
<script>
export default {
  name: 'RoleInfo',
  props: {
    roleInfoVisiable: {
      require: true,
      default: false
    },
    roleInfoData: {
      require: true
    }
  },
  data () {
    return {
      key: +new Date(),
      loading: true,
      checkedKeys: [],
      menuTreeData: []
    }
  },
  methods: {
    close () {
      this.$emit('close')
      this.checkedKeys = []
    }
  },
  watch: {
    roleInfoVisiable () {
      if (this.roleInfoVisiable) {
        this.$get('resource/list').then((r) => {
          this.menuTreeData = r.data.rows.children
          this.$get('sys/role/resource/' + this.roleInfoData.id).then((r) => {
            let length = this.checkedKeys.length
            this.checkedKeys.splice(0, length, r.data.data)
            this.key = +new Date()
          })
        })
      }
    }
  }
}
</script>
