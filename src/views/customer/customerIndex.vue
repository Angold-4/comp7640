<script setup>
import {
  Document,
  Edit,
  ShoppingCart,
  Search,
  ArrowDown,
  Menu as IconMenu, Setting,
} from '@element-plus/icons-vue'
import {ref} from "vue";
import router from "@/router/index.js";
import {useRoute} from "vue-router";
import {useCustomerStore} from "@/stores/index.js";

const customerStore = useCustomerStore()
const route = useRoute()
const searchBox = ref('')

const tagModel = ref({
  tag1: false,
  tag2: false,
  tag3: false,
  tag4: false,
  tag5: false,
  tag6: false,
  tag7: false,
  tag8: false,
})

const chosenTag = ref([])
const handleOpen = (key, keyPath) => {
  console.log(key, keyPath)
}
const handleClose = (key, keyPath) => {
  console.log(key, keyPath)
}


const sendParams = async () => {
  if (route.path === '/cusGoods') {
    await router.push({
      path: '/cusGoods',
      query: {
        productName: searchBox.value,
        tags:chosenTag.value.join(',')
      }
    })
    router.go(0)
  } else if (route.path === '/cusVendors') {
    await router.push({
      path: '/cusVendors',
      query: {
        businessName: searchBox.value
      }
    })
    router.go(0)
  }

}

const exit = () => {
  router.push('/')
  customerStore.removeCustomerId()
}

</script>

<template>
  <div class="common-layout">
    <el-container>
      <el-aside class="leftMenu">
        <el-row v-if="customerStore.customerId">
          <el-col :span="24">
            <el-menu
                active-text-color="#ffd04b"
                background-color="#232323"
                class="el-menu-vertical"
                :default-active="$route.path"
                text-color="#fff"
                @open="handleOpen"
                @close="handleClose"
                router
            >
              <h3 style="color: #fff;text-align: center">Customer</h3>
              <el-menu-item class="item" index="/cusGoods">
                <el-icon>
                  <document/>
                </el-icon>
                <span>Goods List</span>
              </el-menu-item>
              <el-menu-item class="item" index="/cusVendors">
                <el-icon>
                  <icon-menu/>
                </el-icon>
                <span>All vendors</span>
              </el-menu-item>
              <el-menu-item class="item" index="/cart">
                <el-icon>
                  <ShoppingCart/>
                </el-icon>
                <span>Cart</span>
              </el-menu-item>
              <el-menu-item class="item" index="/order">
                <el-icon>
                  <Edit/>
                </el-icon>
                <span>Order Modification</span>
              </el-menu-item>
            </el-menu>
          </el-col>
        </el-row>
        <el-row v-else>
          <el-col :span="24">
            <el-menu
                active-text-color="#ffd04b"
                background-color="#232323"
                class="el-menu-vertical"
                :default-active="$route.path"
                text-color="#fff"
                @open="handleOpen"
                @close="handleClose"
                router
            >
              <h3 style="color: #fff;text-align: center">Vendor</h3>
              <el-menu-item class="item" index="/goods">
                <el-icon>
                  <document/>
                </el-icon>
                <span>Goods List</span>
              </el-menu-item>
              <el-menu-item class="item" index="/vendors">
                <el-icon>
                  <icon-menu/>
                </el-icon>
                <span>All vendors</span>
              </el-menu-item>
              <el-menu-item class="item" index="/introduce">
                <el-icon>
                  <document/>
                </el-icon>
                <span>New vendor</span>
              </el-menu-item>
              <el-menu-item class="item" index="/manage">
                <el-icon>
                  <setting/>
                </el-icon>
                <span>Manage goods</span>
              </el-menu-item>
              <el-menu-item class="item" index="/newGoods">
                <el-icon>
                  <setting/>
                </el-icon>
                <span>New goods</span>
              </el-menu-item>
            </el-menu>
          </el-col>
        </el-row>
      </el-aside>
      <el-container>
        <el-header v-if="route.path === '/cusGoods' || route.path === '/cusVendors'" height="100px">
          <div class="search">
            <el-input v-model="searchBox" style="width: 300px" placeholder="Please input your search" size="large"/>
            <el-button :icon="Search" size="large" @click="sendParams"/>
          </div>
          <div class="tag" v-if="$route.path === '/cusGoods' || $route.path === '/cart'">
            <el-dropdown :hide-on-click="false" size="large">
              <el-button type="primary">
                Choose Tag List
                <el-icon class="el-icon--right">
                  <arrow-down/>
                </el-icon>
              </el-button>
              <template #dropdown>
                <el-dropdown-menu>
                  <el-checkbox-group v-model="chosenTag" :max="3">
                    <el-dropdown-item>
                      <el-checkbox v-model="tagModel.tag1" label="Tag 1" value="1" size="large"/>
                    </el-dropdown-item>
                    <el-dropdown-item>
                      <el-checkbox v-model="tagModel.tag2" label="Tag 2" value="2" size="large"/>
                    </el-dropdown-item>
                    <el-dropdown-item>
                      <el-checkbox v-model="tagModel.tag3" label="Tag 3" value="3" size="large"/>
                    </el-dropdown-item>
                    <el-dropdown-item>
                      <el-checkbox v-model="tagModel.tag4" label="Tag 4" value="4" size="large"/>
                    </el-dropdown-item>
                    <el-dropdown-item>
                      <el-checkbox v-model="tagModel.tag5" label="Tag 5" value="5" size="large"/>
                    </el-dropdown-item>
                    <el-dropdown-item>
                      <el-checkbox v-model="tagModel.tag6" label="Tag 6" value="6" size="large"/>
                    </el-dropdown-item>
                    <el-dropdown-item>
                      <el-checkbox v-model="tagModel.tag7" label="Tag 7" value="7" size="large"/>
                    </el-dropdown-item>
                    <el-dropdown-item>
                      <el-checkbox v-model="tagModel.tag8" label="Tag 8" value="Tag 8" size="large"/>
                    </el-dropdown-item>
                  </el-checkbox-group>
                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </div>
        </el-header>
        <el-main>
          <router-view></router-view>
        </el-main>
        <el-footer>
          <div class="foot">
            <el-button type="primary" @click="exit">Exit</el-button>
          </div>
        </el-footer>
      </el-container>
    </el-container>
  </div>
</template>

<style scoped>
.leftMenu {
  width: 300px;
  height: 100vh;
  font-weight: 400;
  font-size: 25px;
}

.leftMenu .el-menu-vertical {
  height: 100vh;
}

.leftMenu .item {
  width: 400px;
  margin: 10px auto;
  font-size: 17px;
}

.search {
  display: flex;
  width: 100%;
  height: 50%;
  margin: 10px 0;
  align-items: center;
  justify-content: start;
}

.tag {
  width: 400px;
  display: flex;
  justify-content: start;
}

.foot {
  width: 100%;
  padding: 0 30px;
  display: flex;
  justify-content: flex-end;
}

.example-showcase .el-dropdown + .el-dropdown {
  margin-left: 15px;
}

.example-showcase .el-dropdown-link {
  cursor: pointer;
  color: var(--el-color-primary);
  display: flex;
  align-items: center;
}
</style>
