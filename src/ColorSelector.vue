<template>
    <div class="container">

        <div class="header" @click="set_global_id">
            <div class="header_title">{{title}} </div>
            <div class="header_indicator" v-bind:style="get_indicator_css()">{{get_indicator_text()}}</div>  
        </div>
          
        <transition name="fade">    
            <div class="selector" v-if="sel_id == global_id">            
                <div  class="color-radio" v-for="item in items" v-bind:key="item.id">
                    <input v-bind:id = "item.id" v-bind:key="'y_'+item.id"  v-bind:value = "item.value" type="radio" name="radio-color" v-model="local_value"  @change="$emit('input', $event.target.value)">
                    <label v-bind:for = "item.id" v-bind:style="get_unit_css(item.value)">{{get_text(item.data)}}</label>

                </div>      
            </div> 
        </transition>    
    </div>        




</template>
<script>

    export default{
        name: 'colorSelector',
        props: {
            title:String, 
            value:String, 
            type:String, 
            items:Array, 
            sel_id:String,  
            global_id:String,  
            fly_position:Object
        },
        
        data() {
            return{
                local_value: this.value,

            }
        },

        methods: { 
            get_indicator_css: function() {
                
                for (let key in this.items) {
                        if (this.items[key].value == this.value) {
                           if(this.type == 'color'){
                                return {'background':this.items[key].data }
                            }else if(this.type == 'img'){
                                return {backgroundImage:'url('+this.items[key].data+')', backgroundSize:'cover' }
                            } 
                           
                        }
                    }
            },
            get_indicator_text: function(){
                
                for (let key in this.items) {
                        if (this.items[key].value == this.value) {
                            if(this.type == 'color'){
                                return 
                            }else if(this.type == 'img'){
                                return
                            }else if(this.type == 'text'){
                                return this.items[key].data
                            }
                        }
                }
            },
            get_unit_css:function(value){
                for (let key in this.items) {
                        if (this.items[key].value == value) {
                            if(this.type == 'color'){
                                return {'background':this.items[key].data }
                            }else if(this.type == 'img'){
                                return {backgroundImage:'url('+this.items[key].data+')', backgroundSize:'cover' }
                            }
                        }
                }
            },
            set_global_id:function(){
                this.$emit('set_global_id', this.sel_id, this.fly_position);
            },
            
            get_text:function(val){
                if(this.type == 'color'){
                    return 
                }else if(this.type == 'img'){
                    return
                }else if(this.type == 'text'){
                    return val  
                }
            }
        },    
        
        
        

    }

</script>
<style>
    .container{
        align-content:flex-start;
        flex-wrap: wrap;
        align-items: flex-start;
        
    }
    .header{
        display: flex;  
        padding: 5px 0 5px 0;
        width: 100%;
        cursor: pointer;
        justify-content:space-between;
        align-items:center;
        border-bottom:1px solid #D3D3D3;

    }   
    .header_title{
        display: block;  
        

    }
    .header_indicator{
        display: block;  
        height: 25px;
        min-width: 25px;
        border-radius: 0.25rem;

    }
    .selector{
        flex-direction: row;
        align-items: flex-start;
        display: flex;
        flex-wrap:wrap;
        justify-content:flex-start;
        width: 100%;

    }   


    input[type="radio"] {
        display: none;
    }
    input[type="radio"] + label  {
        position:relative;
        z-index:1;
        cursor: pointer;
        display: inline-block;
        height: 40px;
        min-width: 40px;
        padding: 0 5px;
        line-height: 2;
        margin-left: 3px;
         margin-top: 6px;
        border-radius: 0.25rem;
        border: solid 1px #D3D3D3;
        

    }
    input[type="radio"]:checked + label:before  {
        content: '';
        position:absolute;  
        z-index:-1;
        top:-9px;
        right: -7px;
        width:20px;
        height:20px;
        background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACgAAAAiCAYAAAAtZZsLAAAFeklEQVRYR83YW1BUdRwH8O9ZRaapxL0BOlOxmC57ZJelIh/sJQVXbivi7iogjymmWT1kVjQ9NM5khWXaBa2evCArOQyOOoxUD1FRwrpLi9o0SxcTBBbXXdJC4DS/s+cwezm7gFzqP7Nzdva/e/6f/f5v5xwG//PC/Ae+yDa5eIa5BjKDg4N6uVzuHB4e3peYmPg6gDHhJQmdS6Csr6/PoFarHWJigUBg/8KFC6sB3AUwCiAKOVdAwmWp1eoOwjU6K1Gk/xzzZAnw+/0HkpKSXgPwjxRyLoCy/v5+o0qlaifcF5dsuH7rIubJFmDrKhd/9Pl8B+Vy+StSyNkGhuEaHDb0+C+Oz4kI5CG5XL4nEjmbQMJlq1QqXtTgsKLHz4cYVgi57alOyJgEdHd3P5Oenn4MwLDQ3ZgtIOEeU6lUP5LmlMOCXj8//KJKmuJpFOk/4z+vrq5etXfv3i4AfwEYoUkzG0DZwMDA40ql8ocgbiN6/eMTNwyYplyNosxP+c+qqqqstbW1PwEYABAQUpwQyFitVpndbhenPx3jLayEe0KpVLZRo/aOUtwIXJJMTqNcg8LMI3xdZWXllqNHj/4CwAtgEMCQsPTEBTIejydZo9H00D8ym82Lm5qaaGyIC2tkw4TLUSqV3wdxG3Aj4IyBy0Vh5mG+bkt5eeWxEyc8AHwCjtL7W1y8Y3Ux43a7U1iWvQ4ExynHcYGcnJyH2tvb7wjjg6BikXm93icVCsV3QVwJbgRckrh0ZR4KMmv5uoqKii3Hjx/vFnAEFHH8+KPvSAGZrq6uVJ1O9yfVf3l1D5YsWomMlA2EHMrKynq4s7NzfBDTd7xe70qFQvEtnbC+owR9sXCqtShY8QmPKysrq6irq/s1Hk4KGIZrufoyLvfa+RPmat9BRupGHqnX6x9xu900TkYFXGsQtx59gU7p5FQmFKz4mK+z2WwVdrt9QlwkkLly5cpirVZ7jVJpubobl3tPhTW2Rvs2dKkWHsmybNq5c+cy09LSvqYvnWw3o3+IJmF0Wapah/wVH/EVmyyW8vqGht8mSk48i9jFdJRxHEeDc/5XP78Kd0+dZGNrtPugS7US8g7DMPcFccXoH3JL49T5yGc/5OssFkt5wxRwoQkSMMHn872ZlJS0e4wbweFvDBgZI290Wa19C2yqja842V6E/iFaW6PLo+oCrGMP8RWlpaVlp0+fpuRuCelFTQipc4QmuADAgx6P5w2NRrNzIqR+SSWu+Vpx8zatEFK4QqxjD04LF5UggPsBKNxu90ssy24LIrMwMkYry+TLMnURTOwH/A9KSko2NzY2/h6SnF+4IBhfSuKdOXSZmQcgkVIEIHe5XC/q9fqtU0UuSy6GSXeAb7O4uHjzmTNnRNxNYZ2j675J4UITFN+HIhe5XK4X9Ho9n+SR1izcHY2f5PJkM9bq3udxZrN5U1NT0x9CcveEiwRKIp1O5/MGg6FqjBsVkLcle2R58nqs1b1HdVxhYeHms2fPThsnBZREOhyOXUajcXsspDalBHkZ+3lcfn7+pvPnz9NaSrP1npMTU4i5FwMI626Hw/Gc0Wh8NhKpTdmAvIyaWcHFSjAUH4ncaTQad3DcKA63GrFUZUJuxrs8zmQy2Zqbm2n/pk1f3PinNCHirYOxZjolHIbs6OjYkZ2dvZPjxsAwsiAuN9fW3NIy47iJEoyZpNPp3GUwGLYTLi8vz3rhwgW6LJvR5CYag5GJRiaZVFNTk1NfX9/X1tYmdueMdWto41O5JwlF0o7zAADaHumpAF160TXitMecVDKT38OCF7g0JhOEXWe+cHtIMPFWMe7DoKk0NtkxKPWnaHbQi8AEivsAaKqoe+3ieOnPaGozBZxOMJP+7b8eI3tBtp+dKQAAAABJRU5ErkJggg==);
        background-position: center center;
        background-size: cover;
        background-repeat: no-repeat;
        
    }
    
    .fade-enter-active, .fade-leave-active {
                   
        overflow-y: hidden;
        -webkit-transition: max-height 0.5s ease-in-out;
        -moz-transition: max-height 0.5s ease-in-out;
        -o-transition: max-height 0.5s ease-in-out;
        transition: max-height 0.5s ease-in-out;
    }
    .fade-enter, .fade-leave-to {
        max-height: 0; 
    }
</style>    

