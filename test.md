 _   _           _   _       _          _____       _ _           _   _           
| | | |         | | (_)     (_)        /  __ \     | | |         | | (_)          
| | | | ___  ___| |_ _  __ _ _ _ __ ___| /  \/ ___ | | | ___  ___| |_ ___   _____ 
| | | |/ _ \/ __| __| |/ _` | | '__/ _ \ |    / _ \| | |/ _ \/ __| __| \ \ / / _ \
\ \_/ /  __/\__ \ |_| | (_| | | | |  __/ \__/\ (_) | | |  __/ (__| |_| |\ V /  __/
 \___/ \___||___/\__|_|\__,_|_|_|  \___|\____/\___/|_|_|\___|\___|\__|_| \_/ \___|
                                                                                  
                                                                                  
 _____         _           _           _   _____         _                        
|_   _|       | |         (_)         | | |_   _|       | |                       
  | | ___  ___| |__  _ __  _  ___ __ _| |   | | ___  ___| |_                      
  | |/ _ \/ __| '_ \| '_ \| |/ __/ _` | |   | |/ _ \/ __| __|                     
  | |  __/ (__| | | | | | | | (_| (_| | |   | |  __/\__ \ |_                      
  \_/\___|\___|_| |_|_| |_|_|\___\__,_|_|   \_/\___||___/\__|                     
                                                                                  
                                                                                  
______               _   _____          _                                         
|  ___|             | | |  ___|        | |                                        
| |_ _ __ ___  _ __ | |_| |__ _ __   __| |                                        
|  _| '__/ _ \| '_ \| __|  __| '_ \ / _` |                                        
| | | | | (_) | | | | |_| |__| | | | (_| |                                        
\_| |_|  \___/|_| |_|\__\____/_| |_|\__,_|                                        
                                                                                  

## I - Development

Given the products.json that is on this project which represents a response from a getProducts() API call
Please provide a solution for each questions that follows with the stack/framework/language of your choice
Each question is independent from the others.

Comments are appreciated ;-)


1. User is coming from an Off-White promotion offer link, display only the Off-White's products with a reduced price of 10%.	

Assuming the link user redirected from having brandName in query parameters. We would check for brand name Off-White in query params using 
```
var brandName = this.route.snapshot.queryParamsMap.get('brand') === "Off-White";
```

in HTML file we would diplay Discounted field on conditional operator ngIf using brand name 'Off-White'. OR common field like <h4>  ${{discountedPrice(product.price.price, product.brand)}} </h4>  
```
<div class = "content-box" *ngFor = "let product of products ">
        <a routerLink ="/products/{{product.id}}">
            <h3> {{product.name}} - {{product.brand}}</h3>   
            <h4>  €{{discountedPrice(product.price.price, product.brand)}} </h4>
            <h2 *ngIf = "product.brand === 'Off-White'">Discounted rate ${{discountedPrice(product.price.price, product.brand)}}</h2>
        </a>
    </div>
 ```
 implementing function discountedPrice in ts File.
```
discountedPrice(price, brand){    
    return( brand === "Off-White"? (parseInt(price)- parseInt(price) * (10 / 100)).toString() : price); // returns Discounted value using ternary operator
  }
  ```
 

2. Louis Vuitton doesn't want us to display the name of their brand on our website, could you reverse the name of the brand for each LV product to obfuscate their name ?

For obfuscating a string value here we use btoa like base64_encode. SO we create a function checkBrand(brandName) with parameter and check if brandName "Louis Vuitton" then obfuscate:

```
<h3> {{product.name}} - {{checkBrand(product.brand)}} - €{{product.price.price}}</h3>
```

```
 checkBrand(brandName){       
    return( brandName === 'Louis Vuitton'? btoa(brandName) : brandName);
  }
  ```

3. I'm a user from UK and I want to see product between 1500€ and 500€, ordered from the cheaper to the most expensive that are shippable to my country.

We currently check the Country from shippable_countries array on template 

```
<div>
    <div *ngFor = "let product of products " >
        <div *ngIf="product.shippable_countries.indexOf('UK') > -1" class = "content-box" >
            <a routerLink ="/products/{{product.id}}">
                <h3> {{product.name}} - {{checkBrand(product.brand)}} - ${{product.price.price}}</h3>
                <h4>  ${{checkPrice(product.price.price, product.brand)}} </h4>
                <h2 *ngIf = "product.brand === 'Off-White'">Discounted rate ${{checkPrice(product.price.price, product.brand)}}</h2>
            </a>
        </div>        
    </div>
</div>
```

We handle pricerange and sorting from cheaper to expensive within function like below ang call the function as required
```
priceRange(){
    var maxPrice = 1500;
    var minPrice = 500;

    var resultProductData = this.products.filter(a => {
      return (parseInt(a.price.price) >= minPrice && parseInt(a.price.price) <= maxPrice );
    });
    resultProductData.sort(function (a, b) {
      return parseInt(a.price.price) - parseInt( b.price.price);
    });
    this.products = resultProductData;
  }
  ```

4. We want to display how many days/month/year since each products has been deposited on the website (ie: Deposited 1month and 3days ago)

We can use moment precise range plugin in our app
In Our html File add 
```<p>{{calculateDiff(listing.deposited_on)}}</p> ```

sample code:
```
<div class = "content-box">
    <a routerLink = "/products">
    <button> BACK </button>
    </a>
    <h2> {{product.name}}</h2>
    <p> {{product.brand}}</p>
    <p> €{{product.price.price}}</p>
    <p>{{product.deposited_on}}</p>
    <p>{{calculateDiff(product.deposited_on)}}</p>
</div>
```
  
In ts file create function call passing current date parameter
```
calculateDiff(data){
    let date = new Date(data);
    let currentDate = new Date();
    var diff =  moment.preciseDiff(date, currentDate, true); // here we make use of preciseDiff()
    return ( diff.months+ "Months " + diff.days + "Days Ago");
  }
```


## II - Questions

There are no wrong answers, only good opportunities to learn something new.

1. What metrics are essential in term of Speed ?
Ans: Page load time would be the top priority in terms of speed. This is the important factor for any application to keep the user clinged to your site. This is also a major factor considering the buisness where number of users to be visiting the site is more important. if the page load time takes longer it changes the interest of user and also affecting the user experience.


2. Can you name ways to increase speed (perceived or actual load time) ?
Ans: Speed can be increased by perceived time than actual load time. Percieved loading time can be made efficient using loaders, spinners or progress bars this makes user understand that there are content loading and keep user hooked on to the application. Also using lazy loading to load the the images when needed and not to waste the data and time.

3. Could you tell me what are SSR, pre-rendering and Dynamic rendering ?
Ans: SSR: Server Side rendering is displaying the web page at server side instead of rendering at the browser. So each time u route the Page this would compile at server side and be time consuming.
Pre-rendering: This will allow you to compile all pre-selected routes so you can store a fully populated HTML file to a static server. this makes the user experience easy so first you could display a template and pre populated data and show loader for the data  that user is waiting for so the user is aware of data to be populated.


4. You have a bug to fix, you find the file(s) where the bug occurs, the code is a mess, what do you do ?
Ans: In case of Critical Bug on production i would first Fix the bug for it is high priority task. So fix the bug and push the code and later clean the mess to better optimized way. Also understanding the dependencies the code would be having. If the bug is not critical I ill Resolve the bug and clean the mess.

5. What represent FrontEnd to you ?
Ans: Front End to me is having the Best User Experience, User Friendly and light weighted. Keeping user hooked on through the functionalities and design. 

6. What was the last technical challenge you faced and how you did you handle it ?	
Ans: There have been many challenges you face but one of them i would mention here is working with libraries in ember,So we had many condition tweaking the bootstrap datepicker and eventully got buggy due to multiple conditions and validation. So we realised with the number of conditions and validations we were using we spent a lot of time manipulating the datepicker. Instead we could have a custom component made. So key learning here was to understand the complexity of code and get through a reusable code structure.


7. What is the next language/framework/stack you want to learn this year and why ?
Ans: My next move would be towards Python with AI kicking off in market and topic to interest me. Thats my key pick up.

## III - Extra Time

In case you want to have fun 

1. Could you implement the function resolveSudoku() ?
