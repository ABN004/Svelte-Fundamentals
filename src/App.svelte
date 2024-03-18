<script>
// import fs from 'fs-extra'
import jsPDFInvoiceTemplate, { OutputType } from 'jspdf-invoice-template';
import FancyList from './Child.svelte'
import { v4 as uuid } from 'uuid'
import { tick } from 'svelte';

let fancy_list
	let items = [{id:1,text:'Apple',add_to_cart:true}, {id:2,text:'Orange',add_to_cart:false}, {id:3,text:'Grapes',add_to_cart:true}]
    
    console.log(uuid());

	async function handle_create_fruit(evt){
		// evt.preventDefault()
		// console.log(evt.detail);
        let new_fruit_object = evt.detail
        new_fruit_object.id = uuid()
		// items.push(evt.detail)
		// items = items
        // setTimeout(()=>{
        //     items = [...items,new_fruit_object]
        //     fancy_list.clear_input_text()
        //     fancy_list.focus_input()
        // },1000)
       console.log(document.querySelectorAll('.list_items ul li'))
        items = [...items,new_fruit_object]
        fancy_list.clear_input_text()
        fancy_list.focus_input()
        console.log(items);
        await tick()
        console.log(document.querySelectorAll('.list_items ul li'))
	}

    function handle_remove_fruit(evt){
        items = items.filter((single_item)=>single_item.id!=evt.detail)
    }
   
	

	async function handle_create_pdf(){
		let pdfObject = jsPDFInvoiceTemplate(props);
		console.log('pdf_created',pdfObject)
        // OutputType.Save()
	// 	const outputPath = '/public/Invoice 2021.pdf';
	// 	pdfObject.jsPDFDocObject.save(outputPath, { returnPromise: true }).then((v) => {
    //     console.log('PDF saved to ' + outputPath,v);
    // }).catch(err => {
    //     console.error('Error saving PDF:', err);
    // });
	// const pdfBuffer = pdfObject.output(); // Get the PDF content
	// fs.writeFileSync(outputPath, pdfBuffer);


	}
	console.log(OutputType);

	let props = {
    outputType: OutputType.Save,
    returnJsPDFDocObject: true,
    fileName: "Invoice 2021",
    orientationLandscape: false,
    compress: true,
    logo: {
        src: "https://raw.githubusercontent.com/edisonneza/jspdf-invoice-template/demo/images/logo.png",
        type: 'PNG', //optional, when src= data:uri (nodejs case)
        width: 53.33, //aspect ratio = width/height
        height: 26.66,
        margin: {
            top: 0, //negative or positive num, from the current position
            left: 0 //negative or positive num, from the current position
        }
    },
    stamp: {
        inAllPages: true, //by default = false, just in the last page
        src: "https://raw.githubusercontent.com/edisonneza/jspdf-invoice-template/demo/images/qr_code.jpg",
        type: 'JPG', //optional, when src= data:uri (nodejs case)
        width: 20, //aspect ratio = width/height
        height: 20,
        margin: {
            top: 0, //negative or positive num, from the current position
            left: 0 //negative or positive num, from the current position
        }
    },
    business: {
        name: "Business Name",
        address: "Albania, Tirane ish-Dogana, Durres 2001",
        phone: "(+355) 069 11 11 111",
        email: "email@example.com",
        email_1: "info@example.al",
        website: "www.example.al",
    },
    contact: {
        label: "Invoice issued for:",
        name: "Client Name",
        address: "Albania, Tirane, Astir",
        phone: "(+355) 069 22 22 222",
        email: "client@website.al",
        otherInfo: "www.website.al",
    },
    invoice: {
        label: "Invoice #: ",
        num: 19,
        invDate: "Payment Date: 01/01/2021 18:12",
        invGenDate: "Invoice Date: 02/02/2021 10:17",
        headerBorder: false,
        tableBodyBorder: false,
        header: [
          {
            title: "#", 
            style: { 
              width: 10 
            } 
          }, 
          { 
            title: "Title",
            style: {
              width: 30
            } 
          }, 
          { 
            title: "Description",
            style: {
              width: 80
            } 
          }, 
          { title: "Price"},
          { title: "Quantity"},
          { title: "Unit"},
          { title: "Total"}
        ],
        table: Array.from(Array(80), (item, index)=>([
            index + 1,
            "There are many variations ",
            "Lorem Ipsum is simply dummy text dummy text ",
            200.5,
            4.5,
            "m2",
            400.5
        ])),
        additionalRows: [{
            col1: 'Total:',
            col2: '145,250.50',
            col3: 'ALL',
            style: {
                fontSize: 14 //optional, default 12
            }
        },
        {
            col1: 'VAT:',
            col2: '20',
            col3: '%',
            style: {
                fontSize: 10 //optional, default 12
            }
        },
        {
            col1: 'SubTotal:',
            col2: '116,199.90',
            col3: 'ALL',
            style: {
                fontSize: 10 //optional, default 12
            }
        }],
        invDescLabel: "Invoice Note",
        invDesc: "There are many variations of passages of Lorem Ipsum available, but the majority have suffered alteration in some form, by injected humour, or randomised words which don't look even slightly believable. If you are going to use a passage of Lorem Ipsum, you need to be sure there isn't anything embarrassing hidden in the middle of text. All the Lorem Ipsum generators on the Internet tend to repeat predefined chunks as necessary.",
    },
    footer: {
        text: "The invoice is created on a computer and is valid without the signature and stamp.",
    },
    pageEnable: true,
    pageLabel: "Page ",
};
let show_list =true
</script>
{fancy_list?.read_only}
{items.length}
{#if show_list}
<div class="list_items">
    <FancyList bind:this={fancy_list} {items} on:add_fruit={handle_create_fruit} on:remove={handle_remove_fruit}>
        <!-- <span>{prop.text}</span> -->
    </FancyList>
</div>
{/if}
<button on:click={()=>console.log(items)}>check array</button>

<button on:click={handle_create_pdf}>Generate PDF</button>

<button on:click={()=>{show_list=!show_list}}>hide</button>



<style>
    .list_items{
        max-width: 300px;
    }
</style>