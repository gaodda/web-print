<template>
  <div class="print-form" id="print-form-page">
    <el-button type="primary" size="small" class="print-btn no-print" @click="print">打 印</el-button>
    <div class="print-box" ref="printBox">
      <div class="print-box-title">经费支出申报单</div>
      <div class="print-box-company">
        <label>公司名称：</label>
        <label class="print-box-date"><span>年</span><span>月</span><span>日</span></label>
      </div>
      <table>
        <tr>
          <td class="print-label td-label-l">事<span class="print-space print-reason"></span>由</td>
          <td colspan="5" class="print-content">{{bidder.bidSection.bidSectionName}}({{bidder.tenderProject.tenderProjectCode}})</td>
        </tr>
        <tr>
          <td class="print-label td-label-l">收款单位(个人)</td>
          <td colspan="5" class="print-content">{{bidder.name}}</td>
        </tr>
        <tr>
          <td class="print-label td-label-l">开<span class="print-space print-bank"></span>户<span class="print-space print-bank"></span>行</td>
          <td colspan="2" class="print-content">{{bidder.openingBank}}</td>
          <td class="print-label td-label-c">账&nbsp;&nbsp;&nbsp;&nbsp;号</td>
          <td colspan="2" class="print-content">{{bidder.bankAccount}}</td>
        </tr>
        <tr>
          <td class="print-label td-label-l">金<span class="print-space print-money"></span>额(大写)</td>
          <td colspan="4" class="print-content">{{upperShouldReturnAmount}}</td>
          <td class="print-content">
            <span>&yen;</span>
            <span>{{bidder.shouldReturnAmount | filterLowPrice}}</span>
          </td>
        </tr>
        <tr>
          <td rowspan="2" class="print-label td-label-l">开<span class="print-space print-pay"></span>支<span class="print-space print-pay"></span>内<span class="print-space print-pay"></span>容</td>
          <td colspan="2" class="print-label td-label-c">自行填报</td>
          <td colspan="2" class="print-label td-label-c">财务审核</td>
          <td rowspan="2" class="print-label">领款人签字<br/>(网银回单号)</td>
        </tr>
        <tr>
          <td class="print-label td-label-c">票据张数</td>
          <td class="print-label td-label-c">金&nbsp;&nbsp;&nbsp;&nbsp;额</td>
          <td class="print-label td-label-c">票据张数</td>
          <td class="print-label td-label-c">金&nbsp;&nbsp;&nbsp;&nbsp;额</td>
        </tr>
        <tr>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td rowspan="3" class="print-content"></td>
        </tr>
        <tr>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
        </tr>
        <tr>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
        </tr>
        <tr>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-label">财务审核签字</td>
        </tr>
        <tr>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td rowspan="3" class="print-content"></td>
        </tr>
        <tr>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
        </tr>
        <tr>
          <td class="print-label td-label-l">合计</td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
          <td class="print-content"></td>
        </tr>
        <tr>
          <td class="print-label td-label-l">原借</td>
          <td class="print-content"></td>
          <td class="print-label td-label-c">报销</td>
          <td class="print-content"></td>
          <td class="print-label td-label-c">退补</td>
          <td class="print-content"></td>
        </tr>
      </table>
      <div class="print-box-foot">
        <div class="foot-label">
          <span>财务审核人员:</span>
          <span class="print-content">{{bidder.financeAuditName}}</span>
        </div>
        <div class="foot-label">
          <span>财务录入人员:</span>
          <span class="print-content">{{bidder.financeInputName}}</span>
        </div>
        <div class="foot-label">
          <span>招标代理机构:</span>
          <span class="print-content">{{bidder.tenderAgencyName}}</span>
        </div>
        <div class="foot-label">
          <span>项目经理:</span>
          <span class="print-content">{{bidder.creatorName}}</span>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { bidder } from '@/api'
export default {
  name: 'print-form',
  data () {
    return {
      bidder: {
        bidSection: {
          bidSectionName: ''
        },
        tenderProjectCode: '',
        name: '',
        openingBank: '',
        bankAccount: '',
        creatorName: '',
        shouldReturnAmount: '',
        tenderAgencyName: '',
        financeInputName: '',
        financeAuditName: '',
        tenderProject: {}
      },
      upperShouldReturnAmount: ''
    }
  },
  filters: {
    filterLowPrice (val) {
      if (!val) {
        return '0'
      }
      const strArr = (val + '').split('').reverse().join('').replace(/(\d{3})(?=\d)/g, '$1,').split('').reverse()
      return strArr.join('')
    }
  },
  methods: {
    detail () {
      bidder.printForm(this.$route.params.objectId).then((res) => {
        this.bidder = res.data.detail
        this.upperShouldReturnAmount = res.data.upperShouldReturnAmount
      })
    },
    print () {
      bidder.print(this.$route.params.objectId).then((res) => {
        if (res.data.resCode === '0000') {
          this.$print(this.$refs.printBox)
        } else {
          this.$message({
            type: 'error',
            message: '打印失败，请稍后重试!'
          })
        }
      })
    }
  },
  mounted () {
    this.detail()
  }
}
</script>
<style madia="print">
.print-form {
  background: #eee;
  padding-top: 30px;
}
.print-btn {
  margin-right: 20px;
  float: right;
}
/* 打印区域样式 */
.print-box {
  font-family: '华文行楷';
  font-size: 5.5mm;
  width: 240mm;
  padding: 15mm 15mm 13mm 25mm;
  margin: 0 auto;
  background: #fff;
}
.print-label {
  font-size: 5.5mm;
  text-align: center;
}
.print-space {
  display: inline-block;
}
.print-reason {
  width: 25mm;
}
.print-bank {
  width: 10mm;
}
.print-money {
  width: 12mm;
}
.print-pay {
  width: 5mm;
}
.print-content {
  font-family: 'Avenir', sans-serif;
  font-size: 4mm;
  text-align: left;
}
.print-box-title {
  font-size: 8mm;
  letter-spacing: 1.5mm;
  text-align: center;
}
.print-box-company {
  margin: 5mm 0 3mm;
  text-align: left;
  letter-spacing: 0.5mm;
}
.print-box-date {
  float: right;
}
.print-box-date span {
  display: inline-block;
  width: 15mm;
}
.print-box table {
  width: 100%;
  border: 0.5mm solid black;
  border-collapse: collapse;
}
.print-box table tr {
  height: 6mm;
}
.print-box table td {
  height: 6mm;
  border: 0.3mm solid black;
  padding: 1mm;
}
.print-box .td-label-l {
  width: 38mm;
}
.print-box .td-label-c {
  width: 31mm;
}
.print-box .print-box-foot {
  padding-top: 2mm;
}
.print-box .print-box-foot .foot-label {
  float: left;
  width: 25%;
  text-align: left;
}
</style>
