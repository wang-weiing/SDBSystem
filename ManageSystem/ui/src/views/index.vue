<template>
  <page-header-wrapper>
    <template v-slot:content>
      <div class="page-header-content">
        <div class="avatar">
          <a-avatar size="large" :src="avatar"/>
        </div>
        <div class="content">
          <div class="content-title">
            {{ timeFix }}，{{ nickname }}<span class="welcome-text"></span>
          </div>
          <div> {{ postGroup }} | {{ user.dept.deptName }}</div>
        </div>
      </div>
    </template>
    <template v-slot:extraContent>
    </template>
    <!-- 致谢项目 -->
    <div>
      <a-row :gutter="24">
        <a-col :xl="16" :lg="24" :md="24" :sm="24" :xs="24">
          <!-- 项目简介 -->
          <a-card
            style="width:100%"
            title="SDB 简介"
          >
            <p>SDB--Simple things Do Best</p>
            <p>SDB是基于SpringBoot，Spring Security，JWT，Vue 的前后端分离基础开发平台。</p>
            <p>
              拥有用户管理、部门管理、岗位管理、菜单管理、角色管理、字典管理、参数管理、通知公告、操作日志、登录日志、
              在线用户、定时任务、代码生成、系统接口、服务监控、在线构建器、连接池监视等功能。
            </p>
          </a-card>

        </a-col>
        <a-col
          style="padding: 0 12px"
          :xl="8"
          :lg="24"
          :md="24"
          :sm="24"
          :xs="24">
          <!-- 导航 -->
          <!-- 贡献者 -->
          <a-card title="贡献者" :bordered="false" style="margin-bottom: 24px">
            <div class="members">
              <a-row>
                <a-col :span="12" v-for="(item, index) in contributors" :key="index">
                  <a>
                    <a-popover>
                      <template slot="title">
                        <a-space><a-tooltip title="用户名"><a-icon type="user-add" /></a-tooltip>{{ item.name }}</a-space>
                      </template>
                      <template slot="content">
                        <p><a-space><a-tooltip title="邮箱"><a-icon type="mail" /></a-tooltip> {{ item.email }}</a-space></p>
                      </template>
                      <a-avatar size="small" :src="item.avatar"/>
                      <span class="member">{{ item.name }}</span>
                    </a-popover>
                  </a>
                </a-col>
              </a-row>
            </div>
          </a-card>
        </a-col>
      </a-row>
    </div>
  </page-header-wrapper>
</template>

<script>
import { timeFix } from '@/utils/util'
import { mapGetters } from 'vuex'
import { PageHeaderWrapper } from '@/components/ProLayout'
import { getUserProfile } from '@/api/system/user'

export default {
  name: 'Index',
  components: {
    PageHeaderWrapper
  },
  data () {
    return {
      // 贡献者
      contributors: [
        {
          name: '软件架构实验室',
          email: '2222@qq.com'
        },
        {
          name: '段念念',
          email: '111@qq.com'
        }
      ],
      noTitleKey: 'RuoYi',
      timeFix: timeFix(),
      // 用户信息
      user: {
        dept: {
          deptName: ''
        }
      },
      roleGroup: {},
      postGroup: {}
    }
  },
  computed: {
    ...mapGetters([
      'avatar',
      'nickname'
    ])
  },
  created () {
    this.getUser()
  },
  mounted () {
  },
  methods: {
    // 获取用户信息
    getUser () {
      getUserProfile().then(response => {
        this.user = response.data
        this.roleGroup = response.roleGroup
        this.postGroup = response.postGroup
      })
    },
    onSponsorTabChange (key, type) {
      this[type] = key
    }
  }
}
</script>

<style lang="less" scoped>
  @import "./index.less";
  blockquote {
    padding: 0 1em;
    color: #6a737d;
    border-left: 0.25em solid #dfe2e5;
  }
  .project-list {

    .card-title {
      font-size: 0;

      a {
        color: rgba(0, 0, 0, 0.85);
        margin-left: 12px;
        line-height: 24px;
        height: 24px;
        display: inline-block;
        vertical-align: top;
        font-size: 14px;

        &:hover {
          color: #1890ff;
        }
      }
    }

    .card-description {
      color: rgba(0, 0, 0, 0.45);
      height: 66px;
      line-height: 22px;
      overflow: hidden;
    }

    .project-item {
      display: flex;
      margin-top: 8px;
      overflow: hidden;
      font-size: 12px;
      height: 20px;
      line-height: 20px;

      a {
        color: rgba(0, 0, 0, 0.45);
        display: inline-block;
        flex: 1 1 0;

        &:hover {
          color: #1890ff;
        }
      }

      .download {
        color: rgba(0, 0, 0, 0.25);
        flex: 0 0 auto;
        float: right;
      }
    }

    .ant-card-meta-description {
      color: rgba(0, 0, 0, 0.45);
      height: 44px;
      line-height: 22px;
      overflow: hidden;
    }
  }

  .item-group {
    padding: 20px 0 8px 24px;
    font-size: 0;

    a {
      color: rgba(0, 0, 0, 0.65);
      display: inline-block;
      font-size: 14px;
      margin-bottom: 13px;
      width: 25%;
    }
  }

  .members {
    a {
      display: block;
      margin: 12px 0;
      line-height: 24px;
      height: 24px;

      .member {
        font-size: 14px;
        color: rgba(0, 0, 0, .65);
        line-height: 24px;
        max-width: 100px;
        vertical-align: top;
        margin-left: 12px;
        transition: all 0.3s;
        display: inline-block;
      }

      &:hover {
        span {
          color: #1890ff;
        }
      }
    }
  }

  .mobile {

    .project-list {

      .project-card-grid {
        width: 100%;
      }
    }

    .more-info {
      border: 0;
      padding-top: 16px;
      margin: 16px 0 16px;
    }

    .headerContent .title .welcome-text {
      display: none;
    }
  }

</style>
