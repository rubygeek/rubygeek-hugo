---
categories:
- rspec
- testing
date: "2008-04-21T00:00:00Z"
description: ""
title: 'Code Review: Rspec Tests'
---

I am writing a rails application for my mom to enter her bakery orders for her business. The Order page has a form to search or add a new customer, so i can streamline the process as much as possible. In the Order create method, I either get a customer array or a customer ID.

Here's the order create method in the controller: 

```ruby
  def create
    
    new_customer = params[:order][:customer_id].blank? ? true : false
    
    if new_customer
      @customer = Customer.create(params[:customer]) 
      if @customer.valid?
        @customer.orders.create(params[:order])        
        flash[:notice] = 'New customer was added and order was created successfully' 
        redirect_to orders_url
      else
        flash[:notice] = 'Please fill in all required fields'
        render :action => 'new'
      end
    else
      @customer = Customer.find params[:order][:customer_id] 
      @customer.orders.create(params[:order])
      flash[:notice] = 'Order was successfully created.'
      redirect_to orders_url
    end

 end
```

Ok? anybody see a better way to do that?  It took me awhile to figure out the right branching. Now for my rspec tests. This can take two paths -- new customer or existing customer. 

Test for New Customer: 

```ruby
describe OrdersController, "handling POST /orders with a new customer" do

  before do
    @order = mock_model(Order, :to_param => "1")
    
    Order.stub!(:create).and_return(@order)
    
    @params = {:customer_id => nil} 
    
    @customer = mock_model(Customer, :to_param => "1")
    @customer.stub!(:orders).and_return(Order)
    Customer.stub!(:create).and_return(@customer)
  end
  
  def post_with_successful_save
    @customer.should_receive(:valid?).and_return(true)
    post :create, :order => @params
  end
  
  def post_with_failed_save
    @customer.should_receive(:valid?).and_return(false)
    post :create, :order => @params
  end

  it "should create a new customer" do
    Customer.should_receive(:create).with(anything()).and_return(@customer)   
    post_with_successful_save
  end
  
  it "should create a new order" do
    Order.should_receive(:create).with(anything()).and_return(@order)  
    post_with_successful_save
  end

  it "should redirect to the new order on successful save" do
    post_with_successful_save
    response.should redirect_to(orders_url)
  end

  it "should re-render 'new' on failed save" do
    post_with_failed_save
    response.should render_template('new')
  end
end
```

Hows that look? do I catch all the cases for the New Customer scenario?
