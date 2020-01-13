<template>
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
                            <h1  style="color:#1976d2;">324233</h1>
                            <p>Tổng số đã giải quyết</p>
                        </v-flex>
                        <v-flex md3 xs12 text-center>
                            <h1 style="color:#1976d2;">324233</h1>
                            <p>Hoàn thành</p>
                        </v-flex>
                        <v-flex md3 xs12 text-center>
                            <h1 style="color:#1976d2;">324233</h1>
                            <p>Đang xử lý</p>
                        </v-flex>
                        <v-flex md3 xs12 text-center>
                            <h1 style="color:#1976d2;">324233</h1>
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
                    <apexchart type="pie" width="450" :options="chartOptionsSoQuanXa" :series="statisticalSo"></apexchart> 
                </v-card>
            </v-flex>
            <v-flex md4 xs12 class="pa-2">
                <v-card
                    class="pa-2"
                >
                    <div class="" style="height: 38px; overflow: hidden;">
                        <div class="background-triangle-big ">Quận/ Huyện/ Thị xã</div>
                    </div>
                    <apexchart type="pie" width="450" :options="chartOptionsSoQuanXa" :series="statisticalQuan"></apexchart> 
                </v-card>
            </v-flex>
            <v-flex md4 xs12 class="pa-2">
                <v-card
                    class="pa-2"
                >
                    <div class="" style="height: 38px; overflow: hidden;">
                        <div class="background-triangle-big ">Xã/ Phường/ Thị trấn</div>
                    </div>
                    <apexchart type="pie" width="450" :options="chartOptionsSoQuanXa" :series="statisticalXa"></apexchart> 
                </v-card>
            </v-flex>
        </v-layout>
    </v-flex>
    <v-flex xs12 class="pa-2">
        <v-card
        class="pa-2"
        >
            <div class="" style="height: 38px; overflow: hidden;">
                <div class="background-triangle-big ">Thống kê theo tháng</div>
                <v-layout wrap> 
                    <v-flex md6>
                        <div style="display: flex; align-items: center; height: 40px;">
                            <span class="mx-3" style="cursor: pointer;">SỞ BAN NGÀNH</span>
                            <span class="mx-3" style="cursor: pointer;">QUẬN/ HUYỆN/ THỊ XÃ</span>
                            <span class="mx-3" style="cursor: pointer;">XÃ/ PHƯỜNG/ THỊ TRẤN</span>
                        </div>
                    </v-flex>
                    <v-flex md6 class="text-right">
                        <v-select :items="monthList" style="width: 20%; display:inline-block; margin: 0 10px;">
                        </v-select>
                        <v-select :items="yearList" style="width: 20%; display:inline-block; margin: 0 10px;" item-text="name" item-value="value">
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
                    <v-select :items="listThiXa" class="my-2">
                    </v-select>
                </v-flex>
                <v-flex xs12>
                    <apexchart v-if="!isTable" type="bar" height="330" :options="chartOptionsMonth" :series="statisticalMonth"></apexchart>
                    <v-data-table
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
                            <tr>
                                <td>a</td>
                            </tr>
                        </tbody>
                    </template>
                    </v-data-table>
                </v-flex>
            </v-layout>
        </v-card>
    </v-flex>
  </v-layout>
</template>

<script>

export default {
  components: {
    
  },
  data: () => ({
    isTable: false,
    statisticalYear: [44, 55],
    statisticalSo: [44, 55, 12,23, 34,52],
    statisticalQuan: [44, 55, 12,23, 34,52],
    statisticalXa: [44, 55, 12,23, 34,52],
    statisticalMonth: [{
            name: "Hồ sơ nộp trực tiếp",
            data: [44, 55, 41, 64, 22, 43, 21]
          }, {
            name: "Hồ sơ nộp trực tuyến",
            data: [53, 32, 33, 52, 13, 44, 32]
          }],
    chartOptionsYear: {
        labels: ['Xử lý đúng hạn', 'Xử lý quá hạn',],
    },
    chartOptionsMonth: {
        chart: {
            type: 'bar',
            height: 430
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
            categories: ['Xã 01 ', 'Xã 01', 'Xã 01', 'Xã 01', 'Xã 01', 'Xã 01', 'Xã 01'],
        },
    },
    chartOptionsSoQuanXa: {
        labels: ['Trong hạn', 'Quá hạn','Hoàn thành đúng hạn', 'Hoàn thành quá hạn','Xin rút', 'Chờ bổ sung'],
    },
    monthList: ['Tháng 1','Tháng 2','Tháng 3', 'Tháng 4','Tháng 5','Tháng 6','Tháng 7','Tháng 8','Tháng 9','Tháng 10','Tháng 11','Tháng 12',],
    listThiXa: ['Thị Xã Phú Thọ'],
    
  }),
  computed: {
    yearList () {
        let arr = []
        let year = new Date().getFullYear()
        for( let i = 0; i<=10; i++){
            arr.push({name: 'Năm ' + (year - i ), value: year - i})
        }
        return arr

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
</style>