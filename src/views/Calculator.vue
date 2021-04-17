<template>
  <section class="calc">
    <div class="mb-20">
      <span class="menu-item menu-calc"></span>
      <span class="top-title">Calculator</span>
    </div>
    <n-layout row wrap>
      <n-flex xs7>
        <div class="box-wrap mr-10">
          <n-layout row wrap>
            <n-flex xs10>
              <p class="title">Cloud DB for MySQL</p>
            </n-flex>
            <n-flex>
              <button class="btn-wrap" @click="reset">
                <n-icon type="lineIcon-refresh" class="mr-5" />
                Reset
              </button>
            </n-flex>
          </n-layout>
          <hr class="hr-wrap" />
          <div class="content">
            <n-layout row wrap>
              <n-flex xs4>
                <p>Platform Type</p>
                <select class="input-wrap" id="platformType" v-model="formData.platformType">
                  <option v-for="(item, idx) in platformTypeLists" :key="idx + '_platform'" :value="item.value">
                    {{ item.text }}
                  </option>
                </select>
              </n-flex>
              <n-flex xs4>
                <p>Server Type</p>
                <select class="input-wrap" id="serverType" v-model="formData.serverType">
                  <option v-for="(item, idx) in serverTypeLists" :key="idx + '_server'" :value="item.value">
                    {{ item.text }}
                  </option>
                </select>
              </n-flex>
              <n-flex xs4>
                <p>Server Specification</p>
                <select class="input-wrap" id="serverSpec">
                  <option v-for="(item, idx) in serverSpecLists" :key="idx + '_serverSpec'" :value="item.value">
                    {{ item.text }}
                  </option>
                </select>
              </n-flex>
            </n-layout>
            <n-layout row wrap class="mt-30">
              <n-flex xs4>
                <p>No. of Master Servers (units)</p>
                <n-text-field
                  id="masterServerNum"
                  style="width: 80%"
                  type="number"
                  v-model="formData.masterServerNum"
                  placeholder=""
                ></n-text-field>
              </n-flex>
              <n-flex xs4>
                <p>No. of slave servers (units)</p>
                <n-text-field
                  id="slaveServerNum"
                  style="width: 80%"
                  type="number"
                  v-model="formData.slaveServerNum"
                  placeholder=""
                ></n-text-field>
              </n-flex>
              <n-flex xs4>
                <p>Data Storage Type</p>
                <select class="input-wrap" id="dataStorageType">
                  <option v-for="(item, idx) in dataStorageTypeLists" :key="idx + '_storage'" :value="item.value">
                    {{ item.text }}
                  </option>
                </select>
              </n-flex>
            </n-layout>
            <n-layout row wrap class="mt-30">
              <n-flex xs4>
                <p>Data Storage Usage (GB)</p>
                <n-text-field
                  id="dataStorageUsage"
                  style="width: 80%"
                  type="number"
                  v-model="formData.dataStorageUsage"
                  placeholder=""
                ></n-text-field>
              </n-flex>
              <n-flex xs4>
                <p>Backup Storage Usage (GB)</p>
                <n-text-field
                  id="backupStorageUsage"
                  style="width: 80%"
                  type="number"
                  v-model="formData.backupStorageUsage"
                  placeholder=""
                ></n-text-field>
              </n-flex>
            </n-layout>
            <n-layout row wrap class="mt-30">
              <n-flex xs12>
                <div>
                  <span>Total Estimated Cost(Based on 1 month, VAT excluded)</span>
                  <span class="currency">
                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;KRW
                    <span id="result">{{ currencyResult }}</span>
                  </span>
                </div>
              </n-flex>
            </n-layout>
          </div>
          <hr class="hr-wrap" />
          <ul class="list mt-30">
            <li>
              The "high availability support" is selected, two servers (active master DB and standby master DB) is
              created by default.
            </li>
            <li>Usage fee of data storage is charged per 10GB.</li>
            <li>
              The usage of the backup storage is normally similar to [Data Storage Usage x No. of Days for the Backup
              Storage].
            </li>
            <li>
              For more information about the Cloud DB for MySQL service, please refer to
              <a style="color: #14adea" href="https://dev-www.ncloud.com/product/database/cloudDbMysql" target="_blank"
                >Cloud DB for MySQL Service Information</a
              >
            </li>
          </ul>
        </div>
      </n-flex>
      <n-flex xs5>
        <div class="price-pad">
          <div class="toolbar-box">
            <div class="content-pad">
              <div class="price-info">
                <n-layout row wrap>
                  <n-flex xs10>
                    <p class="text" v-if="radio === 1">{{ oneMonth }}</p>
                    <p class="text" v-if="radio === 3">{{ threeMonth }}</p>
                    <p class="text" v-if="radio === 12">{{ oneYear }}</p>
                  </n-flex>
                  <n-flex xs2>
                    <n-btn class="mt-10" @click="reset">
                      <n-icon type="lineIcon-refresh" />
                    </n-btn>
                  </n-flex>
                </n-layout>
                <div class="price-center">
                  <span>
                    KRW&nbsp;&nbsp;&nbsp;&nbsp;
                    {{ radio === 1 ? currencyResult : cost }}
                  </span>
                </div>
                <div class="radio-group">
                  <n-radio-group v-model="radio" @on-change="handleRadioChange">
                    <n-radio :value="1">1 month</n-radio>
                    <n-radio :value="3">3 month</n-radio>
                    <n-radio :value="12">1 year</n-radio>
                  </n-radio-group>
                </div>
              </div>
            </div>
            <n-layout row wrap class="mt-10 ml-50">
              <n-flex xs3>
                <div class="tool">
                  <n-icon type="lineIcon-link" />
                  <div class="tool-text">Link</div>
                </div>
              </n-flex>
              <n-flex xs3>
                <div class="tool">
                  <n-icon type="lineIcon-print" />
                  <div class="tool-text">Print</div>
                </div>
              </n-flex>
              <n-flex xs3>
                <div class="tool">
                  <n-icon type="lineIcon-document" />
                  <div class="tool-text">Excel</div>
                </div>
              </n-flex>
              <n-flex xs3>
                <div class="tool">
                  <n-icon type="lineIcon-album" />
                  <div class="tool-text">Email</div>
                </div>
              </n-flex>
            </n-layout>
            <div class="details-pad">
              <div class="title">
                <span class="category">Database</span>
                <span class="price">{{ cost }}&nbsp;&nbsp;&nbsp;&nbsp;KRW</span>
              </div>
              <div class="view">
                <n-data-table :items="list" :header="headerList" hide-header no-border></n-data-table>
              </div>
              <div class="result">
                <span class="label">KRW&nbsp;&nbsp;&nbsp;&nbsp;{{ cost }}</span>
                <span class="period">({{ radio === 12 ? '1 year' : `${radio} month` }})</span>
                <div class="buttons mt-10">
                  <n-btn class="normal" @click="handleEdit">Edit</n-btn>
                  <n-btn class="ml-5 normal" @click="handleDelete">Delete</n-btn>
                </div>
              </div>
            </div>
          </div>
        </div>
      </n-flex>
    </n-layout>
  </section>
</template>

<script lang="ts">
import Vue from 'vue';
import Component from 'vue-class-component';
import _ from 'lodash';

interface form {
  platformType: string;
  serverType: string;
  serverSpec: string;
  masterServerNum: number;
  slaveServerNum: number;
  dataStorageType: string;
  dataStorageUsage: number;
  backupStorageUsage: number;
}
@Component({
  components: {},
  watch: {
    formData: {
      handler(val) {
        this.calculate();
      },
      deep: true,
    },
    radio: {
      handler(val) {
        this.estimateCost();
      },
      immediate: true,
    },
  },
})
export default class Calculator extends Vue {
  formData: form = {
    platformType: 'vpc',
    serverType: 'highCpu',
    serverSpec: 'v2m4',
    masterServerNum: 0,
    slaveServerNum: 0,
    dataStorageType: 'hdd',
    dataStorageUsage: 0,
    backupStorageUsage: 0,
  };
  currencyResult: number = 0;
  cost: number = 0;
  platformTypeLists: Array<{ [key: string]: any }> = [
    { text: 'VPC', value: 'vpc' },
    { text: 'Classic', value: 'classic' },
  ];
  serverTypeLists: Array<{ [key: string]: any }> = [
    { text: 'High CPU', value: 'highCpu' },
    { text: 'High Memory', value: 'highMemory' },
    { text: 'Standard', value: 'standard' },
  ];
  serverSpecLists: Array<{ [key: string]: any }> = [
    { text: 'vCPU 2EA, Memory 4GB [G2]', value: 'v2m4' },
    { text: 'vCPU 4EA, Memory 8GB [G2]', value: 'v4m8' },
    { text: 'vCPU 8EA, Memory 16GB [G2]', value: 'v8m16' },
    { text: 'vCPU 16EA, Memory 32GB [G2]', value: 'v16m32' },
  ];
  dataStorageTypeLists = [{ text: 'HDD', value: 'hdd' }, { text: 'SSD', value: 'ssd' }];
  price: { [key: string]: any } = {
    highCpu: {
      v2m4: 160,
      v4m8: 320,
      v8m16: 640,
      v16m32: 1280,
    },
  };
  pricePerHour: { [key: string]: any } = {
    hdd: 0.8,
    ssd: 1.6,
  };
  pricePerMonth: { [key: string]: any } = {
    backup: 100,
  };
  oneMonth: string = 'Total Estimated Cost (Based on 1 month, VAT excluded)';
  threeMonth: string = 'Total Estimated Cost (Based on 3 months, VAT excluded)';
  oneYear: string = 'Total Estimated Cost (Based on 1 year, VAT excluded)';
  radio: number = 1;
  headerList = [{ text: 'header1', value: 'h1' }, { text: 'header2', value: 'h2' }];

  get list() {
    return [
      { h1: 'Service', h2: 'Cloud DB for MySQL' },
      { h1: 'Region', h2: 'Korea' },
      { h1: 'Platform Type', h2: this.findLabel(this.platformTypeLists, this.formData.platformType) },
      { h1: 'Server Type', h2: this.findLabel(this.serverTypeLists, this.formData.serverType) },
      { h1: 'Server Specification', h2: this.findLabel(this.serverSpecLists, this.formData.serverSpec) },
      { h1: 'No. of Master Servers', h2: `${this.formData.masterServerNum} units` },
      { h1: 'No. of slave servers(s)', h2: `${this.formData.slaveServerNum} units` },
      { h1: 'Data Storage Type', h2: this.findLabel(this.dataStorageTypeLists, this.formData.dataStorageType) },
      { h1: 'Data Storage Usage', h2: `${this.formData.dataStorageUsage} GB` },
      { h1: 'Backup Storage Usage', h2: `${this.formData.backupStorageUsage} GB` },
    ];
  }

  reset() {
    this.formData.platformType = 'vpc';
    this.formData.serverType = 'cpu';
    this.formData.serverSpec = 'v2m4';
    this.formData.masterServerNum = 0;
    this.formData.slaveServerNum = 0;
    this.formData.dataStorageType = 'hdd';
    this.formData.dataStorageUsage = 0;
    this.formData.backupStorageUsage = 0;
  }

  calculate() {
    const specPrice: number = this.price.highCpu[this.formData.serverSpec];
    const oneServerPrice: number = specPrice * 24 * 30;
    const masterServerNum = parseInt(this.formData.masterServerNum) || 0;
    const slaveServerNum = parseInt(this.formData.slaveServerNum) || 0;
    const dataStorageUsagePrice =
      this.pricePerHour[this.formData.dataStorageType] * 24 * 30 * ((this.formData.dataStorageUsage || 0) / 10);
    const backupStorageUsagePrice = ((this.formData.backupStorageUsage || 0) / 10) * 1000;

    this.currencyResult =
      oneServerPrice * (masterServerNum + slaveServerNum) + dataStorageUsagePrice + backupStorageUsagePrice;
  }

  handleRadioChange(val: number) {
    this.radio = val;
  }

  estimateCost() {
    this.cost = this.currencyResult * this.radio;
  }

  findLabel(list: Array<{ [key: string]: any }>, key: string) {
    const obj = _.find(list, { value: key });

    return obj.text || '';
  }

  handleEdit() {}

  handleDelete() {}
}
</script>

<style lang="less" scoped>
.calc {
  margin-left: 120px;
  text-align: left;
  .menu-item {
    display: inline-block;
    width: 48px;
    height: 48px;
    vertical-align: middle;
  }
  .box-wrap {
    border: 1px solid #d8d8d8;
    border-radius: 4px;
    .title {
      margin: 20px 30px 0 30px;
      font-weight: bold;
      font-size: 17px;
    }
    .content {
      margin: 0 30px;
      .currency {
        color: #14adea;
        font-weight: bold;
      }
      .input-wrap {
        width: 80%;
        padding: 0 15px;
        min-height: 32px;
        border-radius: 4px;
        border: none;
        font-size: 13px;
        line-height: 32px;
        box-sizing: border-box;
        font-family: inherit;
        color: #222;
        box-shadow: 0 0 0 2px transparent inset, 0 0 0 1px #aaa inset;
        transition: 0.2s;
      }
    }
    .list {
      font-size: 14px;
      color: #707070;
      line-height: 1.86;
    }
  }
  .top-title {
    padding-left: 30px;
    font-size: 22px;
    font-weight: 600;
  }
  .btn-wrap {
    margin: 20px 30px 0 30px;
    background-color: #222;
    border-radius: 4px;
    padding: 8px 15px;
    color: #ffffff;
    font-weight: bold;
    border: none;
    font-size: 13px;
  }
  .hr-wrap {
    width: 100%;
    height: 1px;
    background-color: #e6e4e4;
    border: 0;
    margin: 20px 0;
  }

  .price-pad {
    background-color: #f6f7f8;
    height: 100%;
  }
  .toolbar-box {
    width: 50%;
    margin: 0 0 0 20px;
    border: 1px solid #f6f7f8;
    background-color: #fff;
    .content-pad {
      height: 160px;
      background-color: #222;
      border: 1px solid #222;
      .price-info {
        margin: 10px;
        .price-center {
          color: #14adea;
          font-size: 24px;
          line-height: 1.5;
          font-weight: 500;
          margin: 6px 0 14px;
          font-family: Calibre, sans-serif;
        }
        .text {
          font-size: 12px;
          color: #f6f7f8;
          line-height: 1.3;
          font-weight: 400;
        }
        .radio-group {
          color: #f6f7f8;
        }
      }
    }
    .details-pad {
      margin-top: 10px;
      padding: 11px 12px;
      border: 1px solid #dadada;
      .title {
        background: #6e737f;
        color: #fff;
        position: relative;
        padding: 10px 8px 10px 15px;
        display: flex;
        align-items: center;
        .category {
          max-width: 100px;
          font-size: 1rem;
          line-height: 20px;
          font-weight: 400;
          flex: 1;
        }
        .price {
          max-width: 120px;
          font-size: 0.875rem;
          line-height: 1.5;
          font-weight: 400;
          flex: 1;
          text-align: right;
          word-break: keep-all;
        }
      }
      .view {
        margin: 10px 0;
      }
    }
    .result {
      text-align: left;
      .label {
        font-weight: 400;
      }
      .period {
        font-weight: 400;
        color: #707070;
        margin-left: 5px;
      }
      .buttons {
        text-align: right;
        .normal {
          color: #222;
          background-color: #fff;
        }
      }
    }
    .tool {
      cursor: pointer;
      &:hover {
        color: #14adea;
      }
      .tool-text {
        margin-top: 10px;
        font-size: 12px;
        color: #707070;
      }
    }
  }
}

.menu-calc {
  background: url('../../public/quick-calc-new.svg') center no-repeat;
}
.help {
  background: url('../../public/help.svg') center no-repeat;
}

/deep/ .n-radio .n-radio-icon::after {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #14adea;
  border: 1px solid #14adea;
  content: '';
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%) scale(0);
}
/deep/.n-radio-wrapper .n-radio-checked .n-radio-icon {
  border-color: #14adea;
}
/deep/.n-radio.n-radio-wrapper .n-radio-icon:hover {
  border-color: #14adea;
}
/deep/ .n-radio:not(.n-radio-checked):hover .n-radio-icon {
  border: 2px solid #14adea;
}
/deep/ table.tbl {
  tbody {
    tr td {
      background: #fff;
      vertical-align: top;
      position: static;
      border: none;
      border-width: 1px 1px 0 0;
      padding: 2px;
      text-align: start;
    }
    tr td:first-child {
      border-left: none;
    }
    tr.last-row:not(.is-expand) td {
      border-bottom: none;
    }
  }
  .td-box {
    line-height: 14px;
    vertical-align: left;
  }
}
/deep/ table.tbl.noBorder tbody td:last-child {
  border-right: none;
}
</style>
