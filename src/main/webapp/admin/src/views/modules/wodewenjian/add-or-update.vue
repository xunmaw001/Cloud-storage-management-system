<template>
    <div class="addEdit-block">
        <el-form
                class="detail-form-account"
                ref="ruleForm"
                :model="ruleForm"
                :rules="rules"
                label-width="80px"
                :style="{backgroundColor:addEditForm.addEditBoxColor}"
        >
            <el-row>
                                                 <input id="updateId" name="id" type="hidden">
                      <el-col :span="12">
                          <el-form-item class="input" v-if="type!='info'" label="文件名称" prop="wodewenjianName">
                              <el-input v-model="ruleForm.wodewenjianName"
                                        placeholder="文件名称" clearable></el-input>
                          </el-form-item>
                          <div v-else>
                              <el-form-item class="input" label="文件名称" prop="wodewenjianName">
                                  <el-input v-model="ruleForm.wodewenjianName"
                                            placeholder="文件名称" readonly></el-input>
                              </el-form-item>
                          </div>
                      </el-col>
                     <el-col :span="24">
                         <el-form-item class="upload" v-if="type!='info' && !ro.wodewenjianFile" label="文件" prop="wodewenjianFile">
                             <file-upload
                                     tip="点击上传文件"
                                     action="file/upload"
                                     :limit="3"
                                     :multiple="true"
                                     :fileUrls="ruleForm.wodewenjianFile?ruleForm.wodewenjianFile:''"
                                     @change="wodewenjianFileUploadChange"
                             ></file-upload>
                         </el-form-item>
                         <div v-else>
                             <el-form-item v-if="ruleForm.wodewenjianFile" label="文件" prop="wodewenjianFile">
                                 <img style="margin-right:20px;" v-bind:key="index" v-for="(item,index) in ruleForm.wodewenjianFile.split(',')" :src="item" width="100" height="100">
                             </el-form-item>
                         </div>
                     </el-col>
                   <el-col :span="12">
                       <el-form-item class="select" v-if="type!='info'"  label="文件类型" prop="leixingTypes">
                           <el-select v-model="ruleForm.leixingTypes" placeholder="请选择文件类型">
                               <el-option
                                       v-for="(item,index) in leixingTypesSelectSearch"
                                       v-bind:key="index"
                                       :label="item.indexName"
                                       :value="item.codeIndex">
                               </el-option>
                           </el-select>
                       </el-form-item>
                       <div v-else>
                           <el-form-item class="input" label="文件类型" prop="leixingTypes">
                               <el-select v-model="ruleForm.leixingTypes" placeholder="请选择文件类型" readonly>
                                   <el-option
                                           v-for="(item,index) in leixingTypesSelectSearch"
                                           v-bind:key="index"
                                           :label="item.indexName"
                                           :value="item.codeIndex">
                                   </el-option>
                               </el-select>
                           </el-form-item>
                       </div>
                   </el-col>
                     <el-col :span="24">
                         <el-form-item v-if="type!='info'" label="详情信息" prop="wodewenjianContent">
                             <editor
                                     style="min-width: 200px; max-width: 600px;"
                                     v-model="ruleForm.wodewenjianContent"
                                     class="editor"
                                     action="file/upload">
                             </editor>
                         </el-form-item>
                         <div v-else>
                             <el-form-item v-if="ruleForm.wodewenjianContent" label="详情信息" prop="wodewenjianContent">
                                 <span v-html="ruleForm.wodewenjianContent"></span>
                             </el-form-item>
                         </div>
                     </el-col>
                   <el-col :span="12" v-if="1==2">
                       <el-form-item class="select" v-if="type!='info'"  label="状态" prop="zhuangt1Types">
                          <input value='1' v-model="ruleForm.zhuangt1Types">
                       </el-form-item>
                       <div v-else>
                           <el-form-item class="input" label="状态" prop="zhuangt1Types">
                               <input value='1' v-model="ruleForm.zhuangt1Types">
                           </el-form-item>
                       </div>
                   </el-col>
            </el-row>




            </el-row>
            <el-button v-if="type!='info'" type="primary" class="btn-success" @click="onSubmit">提交</el-button>
            <el-button v-if="type!='info'" class="btn-close" @click="back()">取消</el-button>
            <el-button v-if="type=='info'" class="btn-close" @click="back()">返回</el-button>
            </el-form-item>
        </el-form>


    </div>
</template>
<script>
    // 数字，邮件，手机，url，身份证校验
    import { isNumber,isIntNumer,isEmail,isPhone, isMobile,isURL,checkIdCard } from "@/utils/validate";
    export default {
        data() {
            let self = this

            return {
                addEditForm: {"btnSaveFontColor":"#fff","selectFontSize":"14px","btnCancelBorderColor":"#DCDFE6","inputBorderRadius":"4px","inputFontSize":"14px","textareaBgColor":"#fff","btnSaveFontSize":"14px","textareaBorderRadius":"4px","uploadBgColor":"#fff","textareaBorderStyle":"solid","btnCancelWidth":"88px","textareaHeight":"120px","dateBgColor":"#fff","btnSaveBorderRadius":"4px","uploadLableFontSize":"14px","textareaBorderWidth":"1px","inputLableColor":"#606266","addEditBoxColor":"rgba(245, 247, 250, 1)","dateIconFontSize":"14px","btnSaveBgColor":"#409EFF","uploadIconFontColor":"#8c939d","textareaBorderColor":"#DCDFE6","btnCancelBgColor":"#ecf5ff","selectLableColor":"#606266","btnSaveBorderStyle":"solid","dateBorderWidth":"1px","dateLableFontSize":"14px","dateBorderRadius":"4px","btnCancelBorderStyle":"solid","selectLableFontSize":"14px","selectBorderStyle":"solid","selectIconFontColor":"#C0C4CC","btnCancelHeight":"44px","inputHeight":"40px","btnCancelFontColor":"#606266","dateBorderColor":"#DCDFE6","dateIconFontColor":"#C0C4CC","uploadBorderStyle":"solid","dateBorderStyle":"solid","dateLableColor":"#606266","dateFontSize":"14px","inputBorderWidth":"1px","uploadIconFontSize":"28px","selectHeight":"40px","inputFontColor":"#606266","uploadHeight":"148px","textareaLableColor":"#606266","textareaLableFontSize":"14px","btnCancelFontSize":"14px","inputBorderStyle":"solid","btnCancelBorderRadius":"4px","inputBgColor":"#fff","inputLableFontSize":"14px","uploadLableColor":"#606266","uploadBorderRadius":"4px","btnSaveHeight":"44px","selectBgColor":"#fff","btnSaveWidth":"88px","selectIconFontSize":"14px","dateHeight":"40px","selectBorderColor":"#DCDFE6","inputBorderColor":"#DCDFE6","uploadBorderColor":"#DCDFE6","textareaFontColor":"#606266","selectBorderWidth":"1px","dateFontColor":"#606266","btnCancelBorderWidth":"1px","uploadBorderWidth":"1px","textareaFontSize":"14px","selectBorderRadius":"4px","selectFontColor":"#606266","btnSaveBorderColor":"#409EFF","btnSaveBorderWidth":"1px"},
                id: '',
                type: '',
                ro:{
                            yonghuTypes: false,
                            wodewenjianName: false,
                            wodewenjianFile: false,
                            leixingTypes: false,
                            shuangchuanTime: false,
                            wodewenjianContent: false,
                            zhuangt1Types: false,
                            wodewenjianTime: false,
                },
                ruleForm: {
                            yonghuTypes: '',
                            wodewenjianName: '',
                            wodewenjianFile: '',
                            leixingTypes: '',
                            shuangchuanTime: '',
                            wodewenjianContent: '',
                            zhuangt1Types: '',
                            wodewenjianTime: '',
                },
                        yonghuTypesSelectSearch: [],
                        leixingTypesSelectSearch: [],
                        zhuangt1TypesSelectSearch: [],
                rules: {

                        yonghuTypes: [
                        { required: true, message: '用户不能为空', trigger: 'blur' },
                    ],

                        wodewenjianName: [
                        { required: true, message: '文件名称不能为空', trigger: 'blur' },
                    ],

                        wodewenjianFile: [
                        { required: true, message: '文件不能为空', trigger: 'blur' },
                    ],

                        leixingTypes: [
                        { required: true, message: '文件类型不能为空', trigger: 'blur' },
                    ],

                        shuangchuanTime: [
                        { required: true, message: '上传时间不能为空', trigger: 'blur' },
                    ],

                        wodewenjianContent: [
                        { required: true, message: '详情信息不能为空', trigger: 'blur' },
                    ],

                        zhuangt1Types: [
                        { required: true, message: '状态不能为空', trigger: 'blur' },
                    ],

                        wodewenjianTime: [
                        { required: true, message: '创建时间不能为空', trigger: 'blur' },
                    ],
            },

        };
        },
        props: ["parent"],
        computed: {
        },
        created() {
            this.addEditStyleChange()
            this.addEditUploadStyleChange()
        },
        methods: {
            // 下载
            download(file){
                window.open(`${file}`)
            },
            // 初始化
            init(id,type) {
                if (id) {
                    this.id = id;
                    this.type = type;
                }
                if(this.type=='info'||this.type=='else'){
                    this.info(id);
                }else if(this.type=='cross'){
                    var obj = this.$storage.getObj('crossObj');
                    for (var o in obj){

                                 if(o=='yonghuTypes'){
                                     this.ruleForm.yonghuTypes = obj[o];
                                     this.ro.yonghuTypes = true;
                                     continue;
                                 }

                                 if(o=='wodewenjianName'){
                                     this.ruleForm.wodewenjianName = obj[o];
                                     this.ro.wodewenjianName = true;
                                     continue;
                                 }

                                 if(o=='wodewenjianFile'){
                                     this.ruleForm.wodewenjianFile = obj[o];
                                     this.ro.wodewenjianFile = true;
                                     continue;
                                 }

                                 if(o=='leixingTypes'){
                                     this.ruleForm.leixingTypes = obj[o];
                                     this.ro.leixingTypes = true;
                                     continue;
                                 }

                                 if(o=='shuangchuanTime'){
                                     this.ruleForm.shuangchuanTime = obj[o];
                                     this.ro.shuangchuanTime = true;
                                     continue;
                                 }

                                 if(o=='wodewenjianContent'){
                                     this.ruleForm.wodewenjianContent = obj[o];
                                     this.ro.wodewenjianContent = true;
                                     continue;
                                 }

                                 if(o=='zhuangt1Types'){
                                     this.ruleForm.zhuangt1Types = obj[o];
                                     this.ro.zhuangt1Types = true;
                                     continue;
                                 }

                                 if(o=='wodewenjianTime'){
                                     this.ruleForm.wodewenjianTime = obj[o];
                                     this.ro.wodewenjianTime = true;
                                     continue;
                                 }
                    }
                }
                // 获取用户信息
                this.$http({
                    url: `${this.$storage.get('sessionTable')}/session`,
                    method: "get"
                }).then(({ data }) => {
                    if (data && data.code === 0) {
                    var json = data.data;
                } else {
                    this.$message.error(data.msg);
                }
            });


                       /* 查询用户信息*/
                    this.$http({
                        url: `yonghu/page?page=1&limit=100&sort=&order=&dicCode=yonghu_types`,
                        method: "get"
                    }).then(({data}) => {
                        if (data && data.code === 0) {
                          this.yonghuTypesSelectSearch = data.data.list;
                      } else {
                          this.$message.error(data.msg);
                      }
                    });



                       /* 查询文件类型信息*/
                    this.$http({
                        url: `dictionary/page?page=1&limit=100&sort=&order=&dicCode=leixing_types`,
                        method: "get"
                    }).then(({data}) => {
                        if (data && data.code === 0) {
                          this.leixingTypesSelectSearch = data.data.list;
                      } else {
                          this.$message.error(data.msg);
                      }
                    });



                       /* 查询状态信息*/
                    this.$http({
                        url: `dictionary/page?page=1&limit=100&sort=&order=&dicCode=zhuangt1_types`,
                        method: "get"
                    }).then(({data}) => {
                        if (data && data.code === 0) {
                          this.zhuangt1TypesSelectSearch = data.data.list;
                      } else {
                          this.$message.error(data.msg);
                      }
                    });


            },
            // 多级联动参数
            info(id) {
                this.$http({
                    url: `wodewenjian/info/${id}`,
                    method: "get"
                }).then(({ data }) => {
                    if (data && data.code === 0) {
                    this.ruleForm = data.data;
                } else {
                    this.$message.error(data.msg);
                }
            });
            },
            // 提交
            onSubmit() {
                this.$refs["ruleForm"].validate(valid => {
                    if (valid) {
                        this.$http({
                            url: `wodewenjian/${!this.ruleForm.id ? "save" : "update"}`,
                            method: "post",
                            data: this.ruleForm
                        }).then(({ data }) => {
                            if (data && data.code === 0) {
                            this.$message({
                                message: "操作成功",
                                type: "success",
                                duration: 1500,
                                onClose: () => {
                                this.parent.showFlag = true;
                            this.parent.addOrUpdateFlag = false;
                            this.parent.xueshengCrossAddOrUpdateFlag = false;
                            this.parent.search();
                        }
                        });
                        } else {
                            this.$message.error(data.msg);
                        }
                    });
                    }
                });
            },
            // 获取uuid
            getUUID () {
                return new Date().getTime();
            },
            // 返回
            back() {
                this.parent.showFlag = true;
                this.parent.addOrUpdateFlag = false;
                this.parent.xueshengCrossAddOrUpdateFlag = false;
            },
			wodewenjianFileUploadChange(fileUrls) {
			            this.ruleForm.wodewenjianFile = fileUrls;
			             this.addEditUploadStyleChange()
			},

            addEditStyleChange() {
                this.$nextTick(()=>{
                    // input
                    document.querySelectorAll('.addEdit-block .input .el-input__inner').forEach(el=>{
                    el.style.height = this.addEditForm.inputHeight
                el.style.color = this.addEditForm.inputFontColor
                el.style.fontSize = this.addEditForm.inputFontSize
                el.style.borderWidth = this.addEditForm.inputBorderWidth
                el.style.borderStyle = this.addEditForm.inputBorderStyle
                el.style.borderColor = this.addEditForm.inputBorderColor
                el.style.borderRadius = this.addEditForm.inputBorderRadius
                el.style.backgroundColor = this.addEditForm.inputBgColor
            })
                document.querySelectorAll('.addEdit-block .input .el-form-item__label').forEach(el=>{
                    el.style.lineHeight = this.addEditForm.inputHeight
                el.style.color = this.addEditForm.inputLableColor
                el.style.fontSize = this.addEditForm.inputLableFontSize
            })
                // select
                document.querySelectorAll('.addEdit-block .select .el-input__inner').forEach(el=>{
                    el.style.height = this.addEditForm.selectHeight
                el.style.color = this.addEditForm.selectFontColor
                el.style.fontSize = this.addEditForm.selectFontSize
                el.style.borderWidth = this.addEditForm.selectBorderWidth
                el.style.borderStyle = this.addEditForm.selectBorderStyle
                el.style.borderColor = this.addEditForm.selectBorderColor
                el.style.borderRadius = this.addEditForm.selectBorderRadius
                el.style.backgroundColor = this.addEditForm.selectBgColor
            })
                document.querySelectorAll('.addEdit-block .select .el-form-item__label').forEach(el=>{
                    el.style.lineHeight = this.addEditForm.selectHeight
                el.style.color = this.addEditForm.selectLableColor
                el.style.fontSize = this.addEditForm.selectLableFontSize
            })
                document.querySelectorAll('.addEdit-block .select .el-select__caret').forEach(el=>{
                    el.style.color = this.addEditForm.selectIconFontColor
                el.style.fontSize = this.addEditForm.selectIconFontSize
            })
                // date
                document.querySelectorAll('.addEdit-block .date .el-input__inner').forEach(el=>{
                    el.style.height = this.addEditForm.dateHeight
                el.style.color = this.addEditForm.dateFontColor
                el.style.fontSize = this.addEditForm.dateFontSize
                el.style.borderWidth = this.addEditForm.dateBorderWidth
                el.style.borderStyle = this.addEditForm.dateBorderStyle
                el.style.borderColor = this.addEditForm.dateBorderColor
                el.style.borderRadius = this.addEditForm.dateBorderRadius
                el.style.backgroundColor = this.addEditForm.dateBgColor
            })
                document.querySelectorAll('.addEdit-block .date .el-form-item__label').forEach(el=>{
                    el.style.lineHeight = this.addEditForm.dateHeight
                el.style.color = this.addEditForm.dateLableColor
                el.style.fontSize = this.addEditForm.dateLableFontSize
            })
                document.querySelectorAll('.addEdit-block .date .el-input__icon').forEach(el=>{
                    el.style.color = this.addEditForm.dateIconFontColor
                el.style.fontSize = this.addEditForm.dateIconFontSize
                el.style.lineHeight = this.addEditForm.dateHeight
            })
                // upload
                let iconLineHeight = parseInt(this.addEditForm.uploadHeight) - parseInt(this.addEditForm.uploadBorderWidth) * 2 + 'px'
                document.querySelectorAll('.addEdit-block .upload .el-upload--picture-card').forEach(el=>{
                    el.style.width = this.addEditForm.uploadHeight
                el.style.height = this.addEditForm.uploadHeight
                el.style.borderWidth = this.addEditForm.uploadBorderWidth
                el.style.borderStyle = this.addEditForm.uploadBorderStyle
                el.style.borderColor = this.addEditForm.uploadBorderColor
                el.style.borderRadius = this.addEditForm.uploadBorderRadius
                el.style.backgroundColor = this.addEditForm.uploadBgColor
            })
                document.querySelectorAll('.addEdit-block .upload .el-form-item__label').forEach(el=>{
                    el.style.lineHeight = this.addEditForm.uploadHeight
                el.style.color = this.addEditForm.uploadLableColor
                el.style.fontSize = this.addEditForm.uploadLableFontSize
            })
                document.querySelectorAll('.addEdit-block .upload .el-icon-plus').forEach(el=>{
                    el.style.color = this.addEditForm.uploadIconFontColor
                el.style.fontSize = this.addEditForm.uploadIconFontSize
                el.style.lineHeight = iconLineHeight
                el.style.display = 'block'
            })
                // 多文本输入框
                document.querySelectorAll('.addEdit-block .textarea .el-textarea__inner').forEach(el=>{
                    el.style.height = this.addEditForm.textareaHeight
                el.style.color = this.addEditForm.textareaFontColor
                el.style.fontSize = this.addEditForm.textareaFontSize
                el.style.borderWidth = this.addEditForm.textareaBorderWidth
                el.style.borderStyle = this.addEditForm.textareaBorderStyle
                el.style.borderColor = this.addEditForm.textareaBorderColor
                el.style.borderRadius = this.addEditForm.textareaBorderRadius
                el.style.backgroundColor = this.addEditForm.textareaBgColor
            })
                document.querySelectorAll('.addEdit-block .textarea .el-form-item__label').forEach(el=>{
                    // el.style.lineHeight = this.addEditForm.textareaHeight
                    el.style.color = this.addEditForm.textareaLableColor
                el.style.fontSize = this.addEditForm.textareaLableFontSize
            })
                // 保存
                document.querySelectorAll('.addEdit-block .btn .btn-success').forEach(el=>{
                    el.style.width = this.addEditForm.btnSaveWidth
                el.style.height = this.addEditForm.btnSaveHeight
                el.style.color = this.addEditForm.btnSaveFontColor
                el.style.fontSize = this.addEditForm.btnSaveFontSize
                el.style.borderWidth = this.addEditForm.btnSaveBorderWidth
                el.style.borderStyle = this.addEditForm.btnSaveBorderStyle
                el.style.borderColor = this.addEditForm.btnSaveBorderColor
                el.style.borderRadius = this.addEditForm.btnSaveBorderRadius
                el.style.backgroundColor = this.addEditForm.btnSaveBgColor
            })
                // 返回
                document.querySelectorAll('.addEdit-block .btn .btn-close').forEach(el=>{
                    el.style.width = this.addEditForm.btnCancelWidth
                el.style.height = this.addEditForm.btnCancelHeight
                el.style.color = this.addEditForm.btnCancelFontColor
                el.style.fontSize = this.addEditForm.btnCancelFontSize
                el.style.borderWidth = this.addEditForm.btnCancelBorderWidth
                el.style.borderStyle = this.addEditForm.btnCancelBorderStyle
                el.style.borderColor = this.addEditForm.btnCancelBorderColor
                el.style.borderRadius = this.addEditForm.btnCancelBorderRadius
                el.style.backgroundColor = this.addEditForm.btnCancelBgColor
            })
            })
            },
            addEditUploadStyleChange() {
                this.$nextTick(()=>{
                    document.querySelectorAll('.addEdit-block .upload .el-upload-list--picture-card .el-upload-list__item').forEach(el=>{
                    el.style.width = this.addEditForm.uploadHeight
                el.style.height = this.addEditForm.uploadHeight
                el.style.borderWidth = this.addEditForm.uploadBorderWidth
                el.style.borderStyle = this.addEditForm.uploadBorderStyle
                el.style.borderColor = this.addEditForm.uploadBorderColor
                el.style.borderRadius = this.addEditForm.uploadBorderRadius
                el.style.backgroundColor = this.addEditForm.uploadBgColor
            })
            })
            },
        }
    };
</script>
<style lang="scss">
    .editor {
        height: 500px;

    & /deep/ .ql-container {
          height: 310px;
      }
    }
    .amap-wrapper {
        width: 100%;
        height: 500px;
    }
    .search-box {
        position: absolute;
    }
    .addEdit-block {
        margin: -10px;
    }
    .detail-form-account {
        padding: 12px;
    }
    .btn .el-button {
        padding: 0;
    }
</style>
