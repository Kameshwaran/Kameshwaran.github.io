## Array Inquiry
***
Consider the problem, checking the presence of an element in the array. Let us solve this problem using ruby. There are three possible solutions to do it. Let's look into it.

### 1. Iterating over an array
Traversing over all the elements of the array and comparing them with the given element.

```
status = false
array.each do | element |
  status = true if (element == element_to_find)
end
```

### 2. Using 'index' method
'index' method in ruby returns the index of an element in the array if the element is present. Otherwise it returns nil.

```
status = array.index(element_to_find).present?
```

### 3. ArrayInquirer
Finally we need a solution that should solve the problem correctly and efficiently. It should be more readable. Let's see what 'Arrayinquirer' is,
    
```
["code", "brahma"].inquiry.code? => true
["code", "brahma"].inquiry.brahma? => true
["code", "brahma"].inquiry.really? => false
```

A **ArrayInquirer** is a method which solves the above stated problem in very elegant way. The implementation is as follows,

```
class ArrayInquirer < Array
  private
  def method_missing method, *args
    begin
      any? { |element| element.to_s == method[0..-2] }
    rescue NoMethodError
      super method, *args
    end
  end
end
                           
module ArrayInquiry
  def inquiry
    ArrayInquirer.new(self)
  end
end

Array.send(:include, ArrayInquiry)
```
### Explanation

ArrayInquirer extends Array class and implements method_missing. whenever any method is invoked on the array, the method_missing will be called with name and arguments of original call only if the method is not present. Presence of the element is ensured inside the method_missing implementation.