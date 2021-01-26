# openfeign/feign use for calling api -> spring-cloud-starter-openfeign
# Adding a folder or project in same place in intellj
File-> new -> import exiting project -> maven(external)
# devtools use for hotload, normal restart time 9 sec but hotload time could be 2 sec
# beans is same as model, auto generate json response 
# dispatcherServlet  is responsible  for search controller and revert back
# dow is equivalent as repo like interaction with data base
# for define any type data type :  <T> T
# get data type : System.out.println(customUid.getClass());
# for checking String cond : if (customUid.equals("abcd1234")) {}

# using HASH MAP object return multi type JSON
            Map<String, Object> finalData = new HashMap();
            finalData.put("type", "success");
            finalData.put("message", "User updated successfully");
            finalData.put("data", DataLoc);
            return finalData;
            
# using HASH MAP type genric/any return multi type JSON

            Map<String, T> finalData = new HashMap();
            finalData.put("type", (T) "success");
            finalData.put("message", (T) "User deleted successfully");
            finalData.put("data", (T) returnValue); // returnValue is kind of obj
            return (T) finalData;
            
# using model based class return multi type JSON
        ResponseModel responseBody = new ResponseModel("success", "Registered successfully", addedUser);
        return new ResponseEntity(responseBody, responseHeaders, HttpStatus.CREATED);
        
# bean has in default constructor with zero argument, don’t need to write construct in bean

Ex : 
public class Alpha {
}

public class Beta {
  public Beta() {}
}

In Alpha the default constructor is implicit; in Beta it is explicit. Both have default public constructors as per the JavaBean spec


# DAO(data access object) is a kind of bean
