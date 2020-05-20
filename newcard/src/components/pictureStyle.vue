<template>
  <div class="picture">
    <!-- 标题 -->
    <div class="title">
      <div>图片编辑</div>
    </div>
    <!-- 背景色 -->
    <div class="background">
      <div>背景色:</div>
      <div class="btn">
        <el-color-picker v-model="backgroundColor" ref="defultBackgroundColor"></el-color-picker>
        <div class="reset" @click="resetBackgroundColor">重置</div>
      </div>
    </div>
    <!-- 圆角设置 -->
    <div class="setRadius">
      <div class="radius">圆角设置：</div>
      <div class="block">
        <el-slider v-model="radius" show-input :max="80"></el-slider>
      </div>
    </div>
    <!-- 外边距 -->
    <div class="setMargin">
      <div class="margin">外边距：</div>
      <div class="block">
        <el-slider v-model="margin" show-input :max="80"></el-slider>
      </div>
    </div>
    <!-- 内边距 -->
    <div class="setPadding">
      <div class="padding">内边距：</div>
      <div class="block">
        <el-slider v-model="padding" show-input :max="80"></el-slider>
      </div>
    </div>
    <!-- 上传图片列表 -->
    <div class="pictureList">
      <div class="pictureItem" v-for="(i,k) in pictureItemList" :key="k" :index="k">
        <div class="uploadingImg">
          <div
            class="backgroundImgs"
            @click="openPictureVisible( i,k)"
            :style="{'background': `url(${i.pictureUrl}) 0% 0% / 100% 100%  no-repeat `}"
          >
            <i class="el-icon-plus" v-if="!i.pictureUrl"></i>
            <i class="el-icon-plus" v-if="i.pictureUrl" style="color:#fff"></i>
          </div>
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt />
          </el-dialog>
          <!-- <div style="font-size:14px;color:red;width:100px;">{{i.id}}</div>
          <div style="font-size:14px;color:red;width:300px;    overflow: hidden;">{{i.pictureUrl}}</div> -->
          <!-- 图片 -->
          <el-dialog title="图库" :visible.sync="centerDialogVisible" width="860px" center>
            <mapDepot @setPicture="setPicture" />
            <!-- 确定和取消按钮 -->
            <div class="footer">
              <span slot="footer" class="dialog-footer">
                <el-button @click="centerDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="setPictures(i)">确 定</el-button>
              </span>
            </div>
          </el-dialog>
        </div>
        <div class="links">
          链接：
          <!-- <span @click="dialogLIink = true">编辑内容</span> -->
          <el-button type="info" @click="dialogNewLIink(i)">编辑内容</el-button>
        </div>
        <div class="del" @click="delElement(i)"></div>
      </div>
      <!-- 上传图片 -->
      <div class="uploading" @click="addPicture(1)">
        <div>+添加图片</div>
      </div>
      <!-- 链接的对话框 -->
      <el-dialog title="选择链接" :visible.sync="dialogLIink" v-if="dialogLIink" width="70%" center>
        <!-- 中间主体部分 -->
        <Link ref="newOrderDetail" />
        <!-- <el-tabs v-model="activeName" @tab-click="handleClick" type="border-card" size='small'>
                            <el-tab-pane label="商品列表" name="first">
                                <div class="option">
                                    <div class="seek">
                                        <div class="set">
                                            <el-cascader v-model="setValue" @change="handleChangeSet" :options="setOptions" clearable placeholder='全部系列' size='samll'></el-cascader>
                                        </div>
                                        <div class="house">
                                            <el-cascader v-model="houseValue" @change="handleChangeHouse" :options="houseOptions" clearable :props="{ expandTrigger: 'hover' }"></el-cascader>
                                        </div>
                                        <div class="style">
                                            <el-cascader v-model="styleValue" @change="handleChangeStyle" :options="styleOptions" clearable placeholder='全部风格'></el-cascader>
                                        </div>
                                        <div class="tag">
                                            <el-cascader v-model="tagValue" @change="handleChangeTag" :options="tagOptions" clearable placeholder='全部标签'></el-cascader>
                                        </div>
                                        <div class="search">
                                            <el-input placeholder="请输入关键字" v-model="searchInput" clearable></el-input>
                                        </div>
                                    </div>
                                    <div class="bottom">
                                        <el-button type="primary" @click="search">搜 索</el-button>
                                        <el-button type="info" @click="empty">清 空</el-button>
                                    </div>
                                </div>
                                <div class="table">
                                    <el-table ref="multipleTable" :data="tableData" tooltip-effect="dark" style="width: 100%" @selection-change="handleSelectionChange" border >
                                        <el-table-column  type="selection" width="50" align='center'>
                                        </el-table-column>
                                        <el-table-column label="商品名称" width="150" sortable align='center'>
                                            <template  slot-scope="scope">
                                                <div class="productName">
                                                    <div><img :src="scope.row.img" alt=""></div>
                                                    <div class="text">{{ scope.row.name }}</div>
                                                </div>
                                            </template>
                                        </el-table-column>
                                        <el-table-column  prop="set" label="商品系列" align='center'>
                                        </el-table-column>
                                        <el-table-column  prop="classify" label="分类" align='center'>
                                        </el-table-column>
                                        <el-table-column sortable  prop="price" label="价格" align='center'>
                                        </el-table-column>
                                        <el-table-column sortable  prop="inventory" label="库存" align='center'>
                                        </el-table-column>
                                        <el-table-column  prop="state" label="商品状态" align='center'>
                                        </el-table-column>
                                    </el-table>
                                </div>
                                <div class="page">
                                    <el-pagination
                                        @size-change="handleSizeChange"
                                        @current-change="handleCurrentChange"
                                        :current-page="currentPage"
                                        :page-sizes="[5, 10, 15, 20]"
                                        :page-size="5"
                                        layout="total, sizes, prev, pager, next, jumper"
                                        :total="20">
                                    </el-pagination>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="商品标签" name="second">
                                <div class="productLabel">
                                    <div>
                                        <el-radio v-model="labelRadio" label="1" border size="medium">CK系列</el-radio>
                                        <el-radio v-model="labelRadio" label="2" border size="medium">新品</el-radio>
                                        <el-radio v-model="labelRadio" label="3" border size="medium">店长推荐</el-radio>
                                    </div>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="商品系列" name="third">
                                <div class="productSet">
                                    <div>
                                        <el-radio v-model="setRadio" label="1" border size="medium">非意系列</el-radio>
                                        <el-radio v-model="setRadio" label="2" border size="medium">床垫系列</el-radio>
                                        <el-radio v-model="setRadio" label="3" border size="medium">梦之美系列</el-radio>
                                        <el-radio v-model="setRadio" label="4" border size="medium">CK系列</el-radio>
                                        <el-radio v-model="setRadio" label="5" border size="medium">沙发系列</el-radio>
                                    </div>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="商品风格" name="forth">
                                <div class="productStyle">
                                    <div>
                                        <el-radio v-model="styleRadio" label="1" border size="medium">极简</el-radio>
                                        <el-radio v-model="styleRadio" label="2" border size="medium">现代休闲</el-radio>
                                        <el-radio v-model="styleRadio" label="3" border size="medium">北欧</el-radio>
                                        <el-radio v-model="styleRadio" label="4" border size="medium">现代</el-radio>
                                    </div>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="商品类别" name="fifth">
                                <div class="sort"><el-cascader-panel :options="sortOptions"></el-cascader-panel></div>
                            </el-tab-pane>
                            <el-tab-pane label="页面列表" name="sixth">
                                <div class="pageList">
                                    <div class="pageListTop">
                                        <div class="selectAndSeek">
                                            <div class="pageClass">
                                                <el-cascader v-model="pageClassValue" @change="handleChangepageClass" :options="pageClassOptions" clearable placeholder='全部类型' size='samll'></el-cascader>
                                            </div>
                                            <div class="search">
                                                <el-input placeholder="请输入关键字" v-model="pageListsSearchInput" clearable></el-input>
                                            </div>
                                        </div>
                                        <div class="searchBtn">
                                            <el-button type="primary" @click="searchSelect">搜 索</el-button>
                                        </div>
                                    </div>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="固定页面" name="seventh">
                                <div class="fixedPage">
                                    <div>
                                        <el-radio v-model="pageRadio" label="1" border size="medium">720页面</el-radio>
                                        <el-radio v-model="pageRadio" label="2" border size="medium">加密商品</el-radio>
                                        <el-radio v-model="pageRadio" label="3" border size="medium">商场</el-radio>
                                        <el-radio v-model="pageRadio" label="4" border size="medium">官网</el-radio>
                                        <el-radio v-model="pageRadio" label="5" border size="medium">个人中心</el-radio>
                                        <el-radio v-model="pageRadio" label="6" border size="medium">名片</el-radio>
                                        <el-radio v-model="pageRadio" label="7" border size="medium">直播</el-radio>
                                    </div>
                                </div>
                            </el-tab-pane>
                            <el-tab-pane label="表单页面" name="eighth">
                                <div class="formPage">
                                    <div>
                                        <el-radio v-model="formPageRadio" label="1" border size="medium">意向调查</el-radio>
                                        <el-radio v-model="formPageRadio" label="2" border size="medium">意向调查</el-radio>
                                        <el-radio v-model="formPageRadio" label="3" border size="medium">客户调查</el-radio>
                                    </div>
                                </div>
                            </el-tab-pane>
        </el-tabs>-->

        <!-- 页脚 -->
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogLIink = false">取 消</el-button>
          <el-button type="primary" @click="dialogLIink = false">确 定</el-button>
        </span>
      </el-dialog>
      <!-- 图片 -->
      <!-- <el-dialog title="图库" :visible.sync="centerDialogVisible" width="860px" center>
        <mapDepot @setPicture="setPicture" />
        <div class="footer">
          <span slot="footer" class="dialog-footer">
            <el-button @click="centerDialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="setPictures">确 定</el-button>
          </span>
        </div>
      </el-dialog>-->
    </div>
  </div>
</template>

<script>
import Link from "../components/link";
import mapDepot from "../components/mapDepot";
export default {
  components: { Link, mapDepot },
  data() {
    return {
      backgroundColor: "", //背景默认颜色
      radius: 0, //圆角的初始值
      margin: 0, //外边距
      padding: 0, //内边距
      dialogImageUrl: "",
      dialogVisible: false, //上传图片
      centerDialogVisible: false, //上传图片
      pictureBackgroundPicture: "", //图片的选择框的背景图
      defultBackgroundImg: "", //背景图
      pictureItemList: [{ id: 1, pictureUrl: "", pictureLink: "" }],
      num: 1,
      dialogLIink: false, //打开对话框
      pictureObj: {}
    };
  },
  methods: {
    // 重置默认背景颜色
    resetBackgroundColor() {
      this.backgroundColor = "";
      this.$refs.defultBackgroundColor.color.value = this.backgroundColor;
      this.$refs.defultBackgroundColor.$el.firstElementChild.firstElementChild.firstChild.style.backgroundColor = this.backgroundColor;
    },
    // 编辑内容（链接）
    dialogNewLIink(e) {
      this.dialogLIink = true;
      console.log(e);
    },
    // 打开选择图片的弹窗
    openPictureVisible(i, k) {
      this.centerDialogVisible = true;
      console.log(i, k);
    },

    // 点击删除
    delElement(item) {
      for (let i = 0; i < this.pictureItemList.length; i++) {
        if (this.pictureItemList[i] == item) {
          // console.log(i)
          this.pictureItemList.splice(i, 1);
          return;
        }
      }
    },
    // 点击图片改变背景
    setPicture(data) {
      this.defultBackgroundImg = data;
    },
    setPictures(i) {
      this.pictureBackgroundPicture = this.defultBackgroundImg;
      this.centerDialogVisible = false;
    //   console.log(i);
      i.pictureUrl = this.defultBackgroundImg
    //   console.log(this.pictureItemList)
    },
    // 点击添加图片
    addPicture(i) {
      this.pictureObj = { id: this.num + i, pictureUrl: "", pictureLink: "" };
      this.num = this.num + i;
      // console.log(i,this.num)
      this.pictureItemList.push(this.pictureObj);
        console.log(this.pictureItemList)
    }
  },
  watch: {
    // 监听默认背景颜色改变事件
    backgroundColor() {
      this.$emit("getData", this.backgroundColor, "图片");
    },
    // 监听圆角改变
    radius() {
      // console.log(123456789)
      this.$emit("radius", this.radius, "图片");
    },
    // 监听外边距
    margin() {
      this.$emit("margin", this.margin, "图片");
    },
    // 监听内边距
    padding() {
      this.$emit("padding", this.padding, "图片");
    },
    pictureBackgroundPicture() {
      console.log(this.pictureItemList)
      this.$emit(
        "pictureBackgroundPicture", this.pictureBackgroundPicture, "图片");
    },
  }
};
</script>

<style lang="less" scoped>
.picture {
  .title {
    div {
      background-color: #eee;
    }
  }
  .sites,
  .background {
    margin-top: 5px;
    display: flex;
    justify-content: flex-start;
    div {
      width: 90px;
      line-height: 40px;
    }
    .el-input {
      width: 80%;
    }
  }
  .background {
    .btn {
      height: 40px;
      display: flex;
      justify-content: center;
      width: 100%;
      color: #2692ff;
      .m-colorPicker {
        height: 40px;
        padding: 12px 0;
        .colorBtn {
          width: 40px;
          height: 40px;
        }
      }
      .reset {
        width: 30%;
        cursor: pointer;
      }
    }
  }
  .setRadius,
  .setMargin,
  .setPadding {
    .radius,
    .margin,
    .padding {
      background-color: #eee;
    }
    .block {
      padding: 10px 100px;
      z-index: 1;
    }
  }
  .pictureList {
    padding: 10px;
    height: 450px;
    overflow-y: auto;
    .pictureItem {
      position: relative;
      border: 1px solid #e9ebee;
      display: flex;
      padding: 10px;
      margin-top: 10px;
      .uploadingImg {
        width: 30%;
        .backgroundImgs {
          cursor: pointer;
          margin: 0 atut;
          margin-top: 25px;
          margin-left: 20%;
          width: 100px;
          height: 100px;
          background-color: #eee;
          font-size: 40px;
          font-weight: 400;
          border: 1px solid #ddd;
          border-radius: 5px;
          text-align: center;
          display: flex;
          -webkit-box-pack: center;
          justify-content: center;
          -webkit-box-align: center;
          align-items: center;
        }
      }
      .links {
        line-height: 148px;
      }
    }
    .pictureItem:hover {
      border: 1px solid rgb(90, 136, 235);
      .del {
        display: block;
      }
    }
    .del {
      cursor: pointer;
      background: url("../assets/imgs/del.png") no-repeat;
      background-size: 100%;
      width: 20px;
      height: 20px;
      border-radius: 20px;
      position: absolute;
      top: -10px;
      right: -10px;
      display: none;
    }
  }
  .uploading {
    cursor: pointer;
    padding: 10px;
    div {
      height: 34px;
      line-height: 34px;
      border: 1px solid #e9ebee;
      align-items: center;
      margin-top: 10px;
      margin-bottom: 10px;
      color: rgb(90, 136, 235);
    }
  }
  .uploading div:hover {
    border: 1px solid rgb(90, 136, 235);
  }
  .footer {
    display: flex;
    justify-content: center;
  }
  // .option{
  //     display: flex;
  //     justify-content: space-between;
  //     margin-bottom: 20px;
  //     .seek{
  //         display: flex;
  //         justify-content: space-between;
  //         div{
  //             width: 180px;
  //         }
  //         div:not(:first-child){
  //             margin-left: 10px;
  //         }
  //     }
  // }
  // .table{
  //     cursor: pointer;
  // }
  // .page{
  //     margin-top: 20px;
  //     display: flex;
  //     justify-content: center;
  // }
  // .productName{
  //     display: flex;
  //     justify-content: space-between;
  //     img{
  //         width: 40px;
  //         height: 40px;
  //         margin-left: 20px;
  //     }
  //     .text{
  //         margin: 0 auto;
  //         line-height: 40px;
  //         color: #409EFF;
  //     }
  // }
  // .productLabel,.productSet,.productStyle,.fixedPage,.formPage{
  //     margin: 10px 0;
  //     label:not(:first-child){
  //         margin-left: 20px;
  //         margin-top: 5px;
  //     }
  // }
  // .pageList{
  //     .pageListTop{
  //         display: flex;
  //         justify-content: space-between;
  //         .selectAndSeek{
  //             display: flex;
  //             .pageClass{
  //                 width: 180px;
  //                 margin-right: 40px;
  //             }
  //         }
  //     }
  // }
}
</style>