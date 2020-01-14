<template>
    <v-container>
        <v-layout wrap>
            <v-flex xs12 class="pa-2">
                <v-card
                class="pa-2"
                >
                    <div class="row-header" style="height: 38px; overflow: hidden;">
                        <div class="background-triangle-big ">Thống kê hồ sơ theo năm</div>
                        <div style="width: 200px;">
                            <v-select
                                width="200px"
                                :items="yearList"
                                item-value="value"
                                item-text="name"
                                v-model="yearSelected"
                            ></v-select>
                        </div>

                    </div>
                    <v-layout wrap align-center>
                        <v-flex md3 xs 12>
                            <apexchart type="pie" width="400" :options="chartOptionsYear" :series="statisticalYear"></apexchart> 
                        </v-flex>
                        <v-flex md9 xs12>
                            <v-layout wrap>
                                <v-flex md3 xs12 text-center>
                                    <h1  style="color:#1976d2;">{{statistics.processCount}}</h1>
                                    <p>Tổng số đã giải quyết</p>
                                </v-flex>
                                <v-flex md3 xs12 text-center>
                                    <h1 style="color:#1976d2;">{{statistics.releaseCount}}</h1>
                                    <p>Hoàn thành</p>
                                </v-flex>
                                <v-flex md3 xs12 text-center>
                                    <h1 style="color:#1976d2;">{{statistics.processingCount}}</h1>
                                    <p>Đang xử lý</p>
                                </v-flex>
                                <v-flex md3 xs12 text-center>
                                    <h1 style="color:#1976d2;">{{statistics.cancelledCount}}</h1>
                                    <p>Rút hồ sơ</p>
                                </v-flex>
                            </v-layout>
                        </v-flex>
                    </v-layout>
                    
                </v-card>
            </v-flex>
            <v-flex xs12>
                <v-layout wrap>
                    <v-flex md4 xs12 class="pa-2">
                        <v-card
                            class="pa-2"
                        >
                            <div class="" style="height: 38px; overflow: hidden;">
                                <div class="background-triangle-big ">Sở ban ngành</div>
                            </div>
                            <apexchart type="pie" width="400" :options="chartOptionsSoQuanXa" :series="statisticalSBN"></apexchart> 
                        </v-card>
                    </v-flex>
                    <v-flex md4 xs12 class="pa-2">
                        <v-card
                            class="pa-2"
                        >
                            <div class="" style="height: 38px; overflow: hidden;">
                                <div class="background-triangle-big ">Quận/ Huyện/ Thị xã</div>
                            </div>
                            <apexchart type="pie" width="400" :options="chartOptionsSoQuanXa" :series="statisticalQUAN_HUYEN"></apexchart> 
                        </v-card>
                    </v-flex>
                    <v-flex md4 xs12 class="pa-2">
                        <v-card
                            class="pa-2"
                        >
                            <div class="" style="height: 38px; overflow: hidden;">
                                <div class="background-triangle-big ">Xã/ Phường/ Thị trấn</div>
                            </div>
                            <apexchart type="pie" width="400" :options="chartOptionsSoQuanXa" :series="statisticalXA_PHUONG"></apexchart> 
                        </v-card>
                    </v-flex>
                </v-layout>
            </v-flex>
            <v-flex xs12 class="pa-2">
                <v-card
                class="pa-2"
                >
                    <div class="row-header" style="height: 38px; overflow: hidden;">
                        <div class="background-triangle-big ">Thống kê theo tháng</div>
                        <v-layout wrap> 
                            <v-flex md7>
                                <div style="display: flex; align-items: center; height: 40px;">
                                    <span class="mx-3" style="cursor: pointer;" :class="{'groupcode-active':  String(groupCode) === 'SBN'}" @click="groupCode='SBN'">SỞ BAN NGÀNH</span>
                                    <span class="mx-3" style="cursor: pointer;" :class="{'groupcode-active':  String(groupCode) === 'QUAN_HUYEN'}" @click="groupCode='QUAN_HUYEN'">QUẬN/ HUYỆN/ THỊ XÃ</span>
                                    <span class="mx-3" style="cursor: pointer;" :class="{'groupcode-active':  String(groupCode) === 'XA_PHUONG'}" @click="groupCode='XA_PHUONG'">XÃ/ PHƯỜNG/ THỊ TRẤN</span>
                                </div>
                            </v-flex>
                            <v-flex md5 class="text-right">
                                <v-select v-model="monthSelected" :items="monthList" style="width: 27%; display:inline-block; margin: 0 10px;" item-text="name" item-value="value">
                                </v-select>
                                <v-select v-model="yearSelected2" :items="yearList" style="width: 27%; display:inline-block; margin: 0 10px;" item-text="name" item-value="value">
                                </v-select>
                                <v-tooltip bottom>
                                    <template v-slot:activator="{ on }">
                                        <v-btn class="mx-2" :text="isTable" small color="#1976d2" v-on="on"  @click="isTable=false">
                                            <i class="fa fa-pie-chart" aria-hidden="true"></i>
                                        </v-btn>
                                    </template>
                                    <span>Xem dạng biểu đồ</span>
                                </v-tooltip>
                                <v-tooltip bottom>
                                    <template v-slot:activator="{ on }">
                                        <v-btn class="mx-2" :text="!isTable" small color="#1976d2" v-on="on" @click="isTable=true">
                                            <i class="fa fa-table" aria-hidden="true"></i>
                                        </v-btn>
                                    </template>
                                    <span>Xem dạng bảng biểu</span>
                                </v-tooltip>
                            </v-flex>
                        </v-layout>
                    </div>
                    <v-layout wrap>
                        <v-flex md3>
                            <v-select v-model="distGroupSelected" v-if="groupCode==='XA_PHUONG'" :items="listDoiTuong" class="my-2" item-text="itemName" item-value="itemCode">
                            </v-select>
                        </v-flex>
                        <v-flex xs12>
                            <apexchart v-if="!isTable && !isLoading" type="bar" height="330" :options="chartOptionsMonth" :series="statisticalMonth"></apexchart>
                            <v-data-table
                                class="my-2" 
                                v-if="isTable"
                                hide-default-footer
                            >
                                <template v-slot:header="{ props: { headers } }">
                                    <thead>
                                        <tr>
                                            <th rowspan="3"  class="text-center">
                                                <span>STT</span>
                                            </th>
                                            <th rowspan="3" class="text-center">
                                                <span>Đơn vị</span>
                                            </th>
                                            <th colspan="5"  class="text-center">
                                                <span>Nhận giải quyết</span>
                                            </th>
                                            <th colspan="7" class="text-center">
                                                <span>Kết quả nhận giải quyết</span>
                                            </th>
                                            <th colspan="3"  class="text-center">
                                                <span>Đang giải quyết</span>
                                            </th>
                                            <th rowspan="3" style="text-align: center;" width="60">
                                                <span>Tạm dừng bổ sung điều kiện</span>
                                            </th>
                                            <th rowspan="3" style="text-align: center;" width="60">
                                                <span>Rút không giải quyết</span>
                                            </th>
                                            <th rowspan="3" style="text-align: center;" width="60">
                                                <span>Tỉ lệ sớm và đúng hạn</span>
                                            </th>
                                        </tr>
                                        <tr>
                                            <th rowspan="2"  class="text-center">
                                                <span>Tổng số</span>
                                            </th>
                                            <th rowspan="2"  class="text-center">
                                                <span>Tồn trước</span>
                                            </th>
                                            <th colspan="3"  class="text-center">
                                                <span>Nhận trong kì</span>
                                            </th>
                                            <th rowspan="2"  class="text-center">
                                                <span>Tổng số</span>
                                            </th>
                                            <th colspan="3"  class="text-center">
                                                <span>Tình hình thực hiện</span>
                                            </th>
                                            <th rowspan="2"  class="text-center">
                                                <span>Từ chối giải quyết</span>
                                            </th>
                                            <th colspan="2"  class="text-center">
                                                <span>Trả kết quả</span>
                                            </th>
                                            <th rowspan="2"  class="text-center">
                                                <span>Tổng số</span>
                                            </th>
                                            <th rowspan="2"  class="text-center">
                                                <span>Còn hạn</span>
                                            </th>
                                            <th rowspan="2"  class="text-center">
                                                <span>Quá hạn</span>
                                            </th>
                                        </tr>
                                        <tr>
                                            <th  class="text-center">
                                                <span>Tổng số</span>
                                            </th>
                                            <th  class="text-center">
                                                <span>Một cửa</span>
                                            </th>
                                            <th  class="text-center">
                                                <span>Trực tuyến</span>
                                            </th>
                                            <th  class="text-center">
                                                <span>Trước hạn</span>
                                            </th>
                                            <th  class="text-center">
                                                <span>Đúng hạn</span>
                                            </th>
                                            <th  class="text-center">
                                                <span>Quá hạn</span>
                                            </th>
                                            <th  class="text-center">
                                                <span>Đã trả</span>
                                            </th>
                                            <th  class="text-center">
                                                <span>Chưa trả</span>
                                            </th>
                                        </tr>
                                    </thead>
                                </template>
                                <template v-slot:body="{ items }">
                                    <tbody>
                                        <tr class="note__column">
                                            <td align="center">(1)</td>
                                            <td align="center">(2)</td>
                                            <td align="center">(3)</td>
                                            <td align="center">(4)</td>
                                            <td align="center">(5)</td>
                                            <td align="center">(6)</td>
                                            <td align="center">(7)</td>
                                            <td align="center">(8)</td>
                                            <td align="center">(9)</td>
                                            <td align="center">(10)</td>
                                            <td align="center">(11)</td>
                                            <td align="center">(12)</td>
                                            <td align="center">(13)</td>
                                            <td align="center">(14)</td>
                                            <td align="center">(15)</td>
                                            <td align="center">(16)</td>
                                            <td align="center">(17)</td>
                                            <td align="center">(18)</td>
                                            <td align="center">(19)</td>
                                            <td align="center">(20)</td>
                                        </tr>
                                        <tr v-for="(item,index) in danhSachThongKeThang" :key="index">
                                            <td align="center">{{index}}</td>
                                            <td align="left" style="padding: 8px 10px;">{{item.govAgencyName}}</td>
                                            <td align="center">{{item.processCount}}</td>
                                            <td align="center">{{item.remainingCount}}</td>
                                            <td align="center">{{item.receivedCount}}</td>
                                            <td align="center">{{item.onegateCount}}</td>
                                            <td align="center">{{item.onlineCount}}</td>
                                            <td align="center">{{item.releaseCount}}</td>
                                            <td align="center">{{item.betimesCount}}</td>
                                            <td align="center">{{item.ontimeCount}}</td>
                                            <td align="center">{{item.overtimeCount}}</td>
                                            <td align="center">{{item.unresolvedCount}}</td>
                                            <td align="center">{{item.doneCount}}</td>
                                            <td align="center">{{item.releasingCount}}</td>
                                            <td align="center">{{item.processingCount}}</td>
                                            <td align="center">{{item.undueCount}}</td>
                                            <td align="center">{{item.overdueCount}}</td>
                                            <td align="center">{{item.waitingCount}}</td>
                                            <td align="center">{{item.cancelledCount}}</td>
                                            <td align="center">{{item.ontimePercentage}}</td>
                                        </tr>
                                    </tbody>
                                </template>
                            </v-data-table>
                        </v-flex>
                    </v-layout>
                </v-card>
            </v-flex>
        </v-layout>
    </v-container>

</template>

<script>
import axios from 'axios'
export default {
  components: {
    
  },
  data: () => ({
    groupCode: 'SBN',
    isLoading: false,
    yearSelected: new Date().getFullYear(),
    yearSelected2: new Date().getFullYear(),
    monthSelected: new Date().getMonth() + 1,
    distGroupSelected: '',
    isTable: false,
    statisticalYear: [],
    statisticalSBN: [],
    statisticalQUAN_HUYEN: [],
    statisticalXA_PHUONG: [],
    statisticalMonth: [{
            name: "Hồ sơ nộp trực tiếp",
            data: []
          }, {
            name: "Hồ sơ nộp trực tuyến",
            data: []
          }],
    chartOptionsYear: {
        labels: ['Xử lý đúng hạn', 'Xử lý quá hạn',],
    },
    chartOptionsMonth: {
        chart: {
            type: 'bar',
            height: 430,
            locales: [{
                "name": "en",
                "options": {
                    "toolbar": {
                        "exportToSVG": "Tải xuống SVG",
                        "exportToPNG": "Tải xuống PNG"
                    }
                }
            }]
        },
        plotOptions: {
            bar: {
            horizontal: true,
            dataLabels: {
                position: 'top',
            },
            }
        },
        dataLabels: {
            enabled: true,
            offsetX: -6,
            style: {
            fontSize: '12px',
            colors: ['#fff']
            }
        },
        stroke: {
            show: true,
            width: 1,
            colors: ['#fff']
        },
        xaxis: {
            categories: [],
        },
    },
    chartOptionsSoQuanXa: {
        labels: ['Trong hạn', 'Quá hạn','Hoàn thành đúng hạn', 'Hoàn thành quá hạn','Xin rút', 'Chờ bổ sung'],
    },
    monthList: [{name: 'Tháng 1', value: 1},{name: 'Tháng 2', value: 2},{name: 'Tháng 3', value: 3},{name: 'Tháng 4', value: 4},{name: 'Tháng 5', value: 5},{name: 'Tháng 6', value: 6},{name: 'Tháng 7', value: 7},{name: 'Tháng 8', value: 8},{name: 'Tháng 9', value: 9},{name: 'Tháng 10', value: 10},{name: 'Tháng 11', value: 11},{name: 'Tháng 12', value: 12},],
    listDoiTuong: [],
    statistics: {
        betimesCount: 1,
        cancelledCount: 0,
        companyId: 0,
        deniedCount: 0,
        domainCode: '',
        domainName: '',
        doneCount: 21,
        dossierOnline3Count: 0,
        dossierOnline4Count: 0,
        govAgencyCode: '',
        govAgencyName: '',
        groupId: 1,
        insideCount: 0,
        interoperatingCount: 95,
        month: 0,
        onegateCount: 1,
        onlineCount: 10,
        ontimeCount: 2,
        ontimePercentage: 14,
        outsideCount: 0,
        overdueCount: 185,
        overtimeCount: 18,
        overtimeInside: 18,
        overtimeOutside: 0,
        processCount: 241,
        processingCount: 194,
        receiveDossierSatCount: 0,
        receivedCount: 11,
        releaseCount: 21,
        releaseDossierSatCount: 0,
        releasingCount: 0,
        remainingCount: 230,
        reporting: false,
        saturdayCount: 0,
        system: 0,
        totalCount: 241,
        undueCount: 9,
        unresolvedCount: 0,
        viaPostalCount: 0,
        waitingCount: 26,
        year: 2020
    },
    danhSachThongKeThang: []
  }),
  computed: {
    yearList () {
        let arr = []
        let year = new Date().getFullYear()
        for( let i = 0; i<=3; i++){
            arr.push({name: 'Năm ' + (year - i ), value: year - i})
        }
        return arr

    }  
  },
  created () {
    let vm = this;
    this.$nextTick(() => {
        vm.getDictgroups('QUAN_HUYEN')
        vm.getStatisticsYear()
        vm.getStatisticsYearSBN()
        vm.getStatisticsYearQUAN_HUYEN()
        vm.getStatisticsYearXA_PHUONG()
        vm.getStatisticsMonth(vm.groupCode)
    });
  },
  watch: {
    yearSelected () {
        this.getStatisticsYear()
        this.getStatisticsYearSBN()
        this.getStatisticsYearQUAN_HUYEN()
        this.getStatisticsYearXA_PHUONG()
    },
    yearSelected2 () {
        this.getStatisticsMonth(this.groupCode)
    },
    monthSelected () {
        this.getStatisticsMonth(this.groupCode)
    },
    distGroupSelected () {
        this.getStatisticsMonth(this.groupCode)
    },
    groupCode (val) {
        this.getStatisticsMonth(val)
    }
  },
  methods: {
      getDictgroups (key) {
        let vm = this 
        let originUrl =  window.location.origin
        let config = {
            url: originUrl + '/o/rest/v2/dictcollections/REPORT_GROUP/dictgroups/'+ key + '/dictitems',
            headers: {
                groupId : window.themeDisplay.getScopeGroupId(),
                'Accept': 'application/json'
            }
        }
        axios.request(config)
            .then(function (response) {
                if(response.data.data){
                    vm.listDoiTuong  = response.data.data
                    vm.distGroupSelected = response.data.data[0].itemCode
                }

            })
            .catch()
      },
      getStatisticsYear(){
        let vm = this 
        let originUrl =  window.location.origin
        let config = {
            url: originUrl + '/o/rest/statistics',
            headers: {
                groupId : window.themeDisplay.getScopeGroupId(),
                'Accept': 'application/json'
            },
            params: {
                year: vm.yearSelected,
                month: 0
            }
        }
        axios.request(config)
            .then(function (response) {
                if(response.data.data){
                    vm.statistics  = response.data.data[0]
                    vm.statisticalYear = [response.data.data[0].waitingCount, response.data.data[0].ontimeCount]
                }
            })
            .catch()        
      },
      getStatisticsYearSBN(){
        let vm = this 
        let originUrl =  window.location.origin
        let config = {
            url: originUrl + '/o/rest/statistics',
            headers: {
                groupId : window.themeDisplay.getScopeGroupId(),
                'Accept': 'application/json'
            },
            params: {
                year: vm.yearSelected,
                month: 0,
                groupCode: 'SBN'
            }
        }
        axios.request(config)
            .then(function (response) {
                if(response.data.data){
                    vm.statisticalSBN = [response.data.data[0].undueCount, response.data.data[0].overdueCount, response.data.data[0].ontimeCount,  response.data.data[0].overtimeCount,  response.data.data[0].cancelledCount,  response.data.data[0].waitingCount]
                }
                
            })
            .catch()        
      },
      getStatisticsYearQUAN_HUYEN(){
        let vm = this 
        let originUrl =  window.location.origin
        let config = {
            url: originUrl + '/o/rest/statistics',
            headers: {
                groupId : window.themeDisplay.getScopeGroupId(),
                'Accept': 'application/json'
            },
            params: {
                year: vm.yearSelected,
                month: 0,
                groupCode: 'QUAN_HUYEN'
            }
        }
        axios.request(config)
            .then(function (response) {
                if(response.data.data){
                   vm.statisticalQUAN_HUYEN = [response.data.data[0].undueCount, response.data.data[0].overdueCount, response.data.data[0].ontimeCount,  response.data.data[0].overtimeCount,  response.data.data[0].cancelledCount,  response.data.data[0].waitingCount] 
                }
            })
            .catch()        
      },
      getStatisticsYearXA_PHUONG(){
        let vm = this 
        let originUrl =  window.location.origin
        let config = {
            url: originUrl + '/o/rest/statistics',
            headers: {
                groupId : window.themeDisplay.getScopeGroupId(),
                'Accept': 'application/json'
            },
            params: {
                domain: 'total',
                year: vm.yearSelected,
                month: 0,
                groupCode: 'XA_PHUONG'
            }
        }
        axios.request(config)
            .then(function (response) {
                if(response.data.data){
                    vm.statisticalXA_PHUONG = [response.data.data[0].undueCount, response.data.data[0].overdueCount, response.data.data[0].ontimeCount,  response.data.data[0].overtimeCount,  response.data.data[0].cancelledCount,  response.data.data[0].waitingCount]
                }
            })
            .catch()        
      },
      getStatisticsMonth(groupCode){
        let vm = this 
        vm.isLoading = true
        let originUrl =  window.location.origin
        let config = {}
        if(groupCode === 'XA_PHUONG'){
            config = {
                url: originUrl + '/o/rest/statistics',
                headers: {
                    groupId : window.themeDisplay.getScopeGroupId(),
                    'Accept': 'application/json'
                },
                params: {
                    domain: 'total',
                    year: vm.yearSelected2,
                    month: vm.monthSelected,
                    groupCode: groupCode,
                    parentAgency: vm.distGroupSelected,
                }
            }
        } else {
            config = {
                url: originUrl + '/o/rest/statistics',
                headers: {
                    groupId : window.themeDisplay.getScopeGroupId(),
                    'Accept': 'application/json'
                },
                params: {
                    domain: 'total',
                    year: vm.yearSelected2,
                    month: vm.monthSelected,
                    groupCode: groupCode,
                }
            }           
        }

        axios.request(config)
            .then(function (response) {
                vm.statisticalMonth[0].data=[]
                vm.statisticalMonth[1].data=[]
                vm.chartOptionsMonth.xaxis.categories=[]
                if(response.data.data){
                    vm.danhSachThongKeThang = response.data.data
                    for(let i =0 ; i< response.data.data.length ;i++)
                    {
                        
                        vm.statisticalMonth[0].data.push(response.data.data[i+1].onegateCount)
                        vm.statisticalMonth[1].data.push(response.data.data[i+1].onlineCount)
                        vm.chartOptionsMonth.xaxis.categories.push(response.data.data[i+1].govAgencyName)
                        vm.isLoading = false
                    }
                }

            })
            .catch()        
      }
  }
};
</script>
<style>
.row-header{
  height: 38px;
  overflow: hidden;
  display: flex;
  align-items: center;
  width: 100%;
}
.background-triangle-big{
    background-color: #00204A;
    display: inline-block;
    text-align: left;
    color: #fff;
    position: relative;
    margin-right: 20px;
    text-transform: uppercase;
    padding: 9px 15px;
    width: auto;
    cursor: pointer;
}
.background-triangle-big:before, .background-triangle-small:before {
content: "";
    position: absolute;
    transform: rotate(45deg);
    right: -4px;
    top: 2px;
    border-top: 26px solid #00204A;
    border-left: 26px solid transparent;
    border-bottom: 25px solid transparent;
}
.align-center{
    align-items: center;
}
.v-input__slot{
    margin-bottom: 0px!important;
}
.v-text-field {
    padding-top: 0px!important; 
    margin-top: 0px!important; 
}
.v-text-field__details{
    display: none!important;
}
th{
    border: 0.3px solid!important;
}
td{
    border: 0.3px solid!important;
}
.theme--light.v-data-table thead tr:last-child th {
    border-bottom: 0.3px solid!important;
}
.v-card{
    box-shadow: 0px 3px 1px -2px rgba(0, 0, 0, 0.2), 0px 2px 2px 0px rgba(0, 0, 0, 0.14), 0px 1px 5px 0px rgba(0, 0, 0, 0.12)!important;
}
#fragment-501564-1 . {
    background-color: transparent;
}
#fragment-501564-1 .v-list .v-list-item__content > div {
    padding: 10px 15px!important;
}
.groupcode-active{
    color: #1976d2;
}
.position-right{
    right: 73px!important;
}
</style>