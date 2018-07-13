<template>
  <div style="width: 100%;">
    <el-row :gutter="20" type="flex" justify="center" style="margin:0px 10px;">
      <el-col :span="12" :xs="0" :sm="4" :md="4" :lg="8" :xl="12">&nbsp;</el-col>
      <el-col :span="8" :xs="24" :sm="16" :md="16" :lg="12" :xl="8" style="margin-top: 20px;">
        <el-input v-model="searchForm.keyword" placeholder="请输入内容搜索接口">
          <el-button slot="append" icon="el-icon-search"></el-button>
        </el-input>
      </el-col>
      <el-col :span="4" :xs="24" :sm="4" :md="4" :lg="4" :xl="4" style="margin-top: 20px;">
        <el-select v-model="searchForm.project" placeholder="项目">
          <el-option
            v-for="item in projects"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-col>
    </el-row>
    <div style="margin:0px 20px">
      <h3>接口名称</h3>
      <p>接口说明或者接口描述</p>
    </div>
    <el-row :gutter="20" style="margin:0px 10px;">
      <el-col :span="16" :xs="24" style="margin-top: 20px;">
        <el-input placeholder="请输入内容" v-model="reqObj.url">
          <el-select v-model="reqObj.method" slot="prepend" placeholder="请选择" style="width: 100px;">
            <el-option label="GET" value="1"></el-option>
            <el-option label="POST" value="2"></el-option>
            <el-option label="DELETE" value="3"></el-option>
          </el-select>
          <el-button slot="append">/app/restful/get</el-button>
        </el-input>
      </el-col>
      <el-col :span="8" :xs="24" style="margin-top: 20px;">
        <el-button type="primary" @click="onSubmit">发送</el-button>
        <el-button type="success">保存</el-button>
        <el-button type="info">历史</el-button>
      </el-col>
    </el-row>

    <el-row :gutter="10" style="margin:0px 10px;">
      <el-col :span="12" :xs="24" :sm="12" style="margin-top: 20px;">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span style="line-height: 30px;">请求参数</span>
            <el-radio-group v-model="reqObj.bodyType" style="float: right;">
              <el-radio-button label="FROM">FROM</el-radio-button>
              <el-radio-button label="JSON">JSON</el-radio-button>
              <el-radio-button label="RESTFUL">RESTFUL</el-radio-button>
            </el-radio-group>
          </div>
          <el-tabs>
            <el-tab-pane label="HEADER" name="first">
              <h4>请求Header头部</h4>
              <div v-for="(item, index) in reqHeaders" :key="index" style="margin-top: 10px;">
                <el-input v-model="item.key" style="display: inline-block; width: auto;" size="mini"></el-input>
                =
                <el-input v-model="item.value" style="display: inline-block;width: auto;" size="mini"></el-input>
                <el-checkbox v-model="item.enabled" class="m-l-10"></el-checkbox>
                <i class="el-icon-plus pointer m-l-10" @click="addHeader"></i>
                <i class="el-icon-delete pointer m-l-10" @click="removeHeader(item)" v-show="index !== 0"></i>
              </div>
            </el-tab-pane>
            <el-tab-pane label="FORM" name="second"></el-tab-pane>
            <el-tab-pane label="JSON" name="third">
              <h4>JSON格式</h4>
              <div class="custom-tree-container">
                <div class="block">
                  <el-tree
                    :data="json"
                    show-checkbox
                    node-key="id"
                    default-expand-all
                    :expand-on-click-node="false"
                    :render-content="renderContent">
                  </el-tree>
                </div>
              </div>
            </el-tab-pane>
            <el-tab-pane label="QUERY" name="fourth">角色管理</el-tab-pane>
            <el-tab-pane label="RAW" name="five">定时任务补偿</el-tab-pane>
          </el-tabs>
        </el-card>
      </el-col>
      <el-col :span="12" :xs="24" :sm="12" style="margin-top: 20px;">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>返回结果</span>
          </div>
          <el-tabs>
            <el-tab-pane label="返回头部" name="first">用户管理</el-tab-pane>
            <el-tab-pane label="返回内容" name="second">配置管理</el-tab-pane>
            <el-tab-pane label="JSON格式化" name="third">角色管理</el-tab-pane>
          </el-tabs>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "testRun",
  data() {
    return {
      // 请求头
      reqHeaders: [
        {
          key: "",
          value: "",
          enabled: true
        }
      ],
      form: {},
      // json数据
      json: [
        {
          id: 1,
          label: "一级 1",
          children: [
            {
              id: 4,
              label: "二级 1-1",
              children: [
                {
                  id: 9,
                  label: "三级 1-1-1"
                },
                {
                  id: 10,
                  label: "三级 1-1-2"
                }
              ]
            }
          ]
        },
        {
          id: 2,
          label: "一级 2",
          children: [
            {
              id: 5,
              label: "二级 2-1"
            },
            {
              id: 6,
              label: "二级 2-2"
            }
          ]
        },
        {
          id: 3,
          label: "一级 3",
          children: [
            {
              id: 7,
              label: "二级 3-1"
            },
            {
              id: 8,
              label: "二级 3-2"
            }
          ]
        }
      ],
      reqObj: { url: "", method: "1", bodyType: "" },
      searchForm: { keyword: "", project: 0 },
      projects: [{ value: 0, label: "NPJT" }, { value: 1, label: "PJD" }]
    };
  },
  methods: {
    onSubmit() {
      alert(JSON.stringify(this.reqHeaders));
    },
    addHeader() {
      this.reqHeaders.push({
        key: "",
        value: "",
        enabled: true
      });
    },
    removeHeader(reqHeader) {
      const index = this.reqHeaders.indexOf(reqHeader);
      if (index !== -1) {
        this.reqHeaders.splice(index, 1);
      }
    },
    renderContent(h, { node, data, store }) {
      return (
        <span class="custom-tree-node">
          <span>{node.label}</span>
          <el-input v-model="node.value" size="mini" clearable style="margin: auto 10px"></el-input>
          <span>
            <el-button
              size="mini"
              type="text"
              on-click={() => this.append(data)}
            >
              Append
            </el-button>
            <el-button
              size="mini"
              type="text"
              on-click={() => this.remove(node, data)}
            >
              Delete
            </el-button>
          </span>
        </span>
      );
    }
  }
};
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
@import "src/styles/mixin.scss";
.pointer {
  cursor: pointer;
}

.m-l-10 {
  margin-left: 10px;
}
</style>

<style rel="stylesheet/scss" lang="scss">
.custom-tree-node {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 14px;
  padding-right: 8px;
}
</style>

