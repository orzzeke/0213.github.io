<template>
<div>
<div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
      ref="modal"
    >
      <div class="modal-dialog modal-xl" role="document">
        <div class="modal-content border-0">
          <div class="modal-header bg-dark text-white">
            <h5 class="modal-title" id="exampleModalLabel">
              <span>新增產品</span>
            </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <div class="row">
              <div class="col-sm-4">
                <div class="mb-3">
                  <label for="image" class="form-label">輸入圖片網址</label>
                  <input
                    type="text"
                    class="form-control"
                    id="image"
                    v-model="tempProduct.imageUrl"
                    placeholder="請輸入圖片連結"
                  />
                </div>
                <div class="mb-3">
                  <label for="customFile" class="form-label"
                    >或 上傳圖片
                    <i class="fas fa-spinner fa-spin"></i>
                  </label>
                  <input
                    type="file"
                    id="customFile"
                    class="form-control"
                    ref="fileInput"
                    @change="uploadFile"
                  />
                  <!-- <input
                    type="file"
                    id="customFile2"
                    class="form-control"
                    ref="files2"
                    @change="Imgfn2"
                  />
                  <input
                    type="file"
                    id="customFile3"
                    class="form-control"
                    ref="files3"
                    @change="Imgfn3"
                  /> -->
                </div>
                <img class="img-fluid" :src="tempProduct.imageUrl" alt="" />
                <!-- 延伸技巧，多圖 -->
                <div class="mt-5" v-if="tempProduct.images">
                  <div
                    v-for="(image, key) in tempProduct.images"
                    :key="key"
                    class="mb-3 input-group"
                  >
                    <img class="img-fluid" :src="image" alt="" />
                    <input
                      type="url"
                      class="form-control form-control"
                      v-model="tempProduct.images[key]"
                      placeholder="請輸入連結"
                    />
                    <button
                      type="button"
                      class="btn btn-outline-danger"
                      @click="tempProduct.images.splice(key, 1)"
                    >
                      移除
                    </button>
                  </div>
                  <div
                    v-if="
                      tempProduct.images[tempProduct.images.length - 1] ||
                      !tempProduct.images.length
                    "
                  >
                    <button
                      class="btn btn-outline-primary btn-sm d-block w-100"
                      @click="tempProduct.images.push('')"
                    >
                      新增圖片
                    </button>
                  </div>
                </div>
              </div>
              <div class="col-sm-8">
                <div class="mb-3">
                  <label for="title" class="form-label">標題</label>
                  <input
                    type="text"
                    class="form-control"
                    id="title"
                    v-model="tempProduct.title"
                    placeholder="請輸入標題"
                  />
                </div>

                <div class="row gx-2">
                  <div class="mb-3 col-md-6">
                    <label for="category" class="form-label">分類</label>
                    <input
                      type="text"
                      class="form-control"
                      id="category"
                      v-model="tempProduct.category"
                      placeholder="請輸入分類"
                    />
                  </div>
                  <div class="mb-3 col-md-6">
                    <label for="price" class="form-label">單位</label>
                    <input
                      type="text"
                      class="form-control"
                      id="unit"
                      v-model="tempProduct.unit"
                      placeholder="請輸入單位"
                    />
                  </div>
                </div>

                <div class="row gx-2">
                  <div class="mb-3 col-md-6">
                    <label for="origin_price" class="form-label">原價</label>
                    <input
                      type="number"
                      class="form-control"
                      id="origin_price"
                      v-model="tempProduct.origin_price"
                      placeholder="請輸入原價"
                    />
                  </div>
                  <div class="mb-3 col-md-6">
                    <label for="price" class="form-label">售價</label>
                    <input
                      type="number"
                      class="form-control"
                      id="price"
                      v-model="tempProduct.price"
                      placeholder="請輸入售價"
                    />
                  </div>
                </div>
                <hr />

                <div class="mb-3">
                  <label for="description" class="form-label">產品描述</label>
                  <textarea
                    type="text"
                    class="form-control"
                    id="description"
                    v-model="tempProduct.description"
                    placeholder="請輸入產品描述"
                  ></textarea>
                </div>
                <div class="mb-3">
                  <label for="content" class="form-label">說明內容</label>
                  <textarea
                    type="text"
                    class="form-control"
                    id="content"
                    v-model="tempProduct.content"
                    placeholder="請輸入產品說明內容"
                  ></textarea>
                </div>
                <div class="mb-3">
                  <div class="form-check">
                    <input
                      class="form-check-input"
                      type="checkbox"
                      v-model="tempProduct.is_enabled"
                      :true-value="1"
                      :false-value="0"
                      id="is_enabled"
                    />
                    <label class="form-check-label" for="is_enabled">
                      是否啟用
                    </label>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-outline-secondary"
              data-bs-dismiss="modal"
            >
              取消
            </button>
            <button
              type="button"
              class="btn btn-primary"
              @click="$emit('update-product', tempProduct)"
            >
              確認
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import mixin from '@/mixins/ModalMixin'

export default {
  props: {
    product: {
      type: Object,
      default () { return {} }
    }
  },
  // 監聽只要裡面數值有改即變更
  watch: {
    product () {
      // 看product 數值有改就把他放進temp裡面
      this.tempProduct = this.product
    }
  },
  data () {
    return {
      modal: {},
      tempProduct: {}
    }
  },
  methods: {
    uploadFile () {
      const uploadFile = this.$refs.fileInput.files[0]
      const formData = new FormData()
      // append是我們要新增一個欄位到裡面 (欄位名稱 , 所帶的內容)
      formData.append('file-to-upload', uploadFile)
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/upload`
      this.$http.post(url, formData).then((res) => {
        // 如果上傳成功的話就執行把url存進去
        if (res.data.success) {
          this.tempProduct.imageUrl = res.data.imageUrl
        }
      })
    }
  },
  mixins: [mixin]
}
</script>
