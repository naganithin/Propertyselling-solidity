pragma solidity >=0.5.10 <= 0.6.2;

contract PropertyTransfer{
    
    enum PropertyStatus{
        Forbidden,
        Sold,
        ReadyToSell
    }
    
    struct Property{
        int256 PropID;
        string PropName;
        string PropAddress;
        PropertyStatus ProStatus;
        address owner;
    }    

    
    // Fields of the class/contract
    
    mapping (int256 => Property) properties;
    int256 propCount;
    
    constructor() public {
        propCount = 0;
    }
    
    // the below function verifies whether the current user is the owener 
    function IsOwner(address paramPropOwner) private view returns (bool paramisOwner){
        return(msg.sender == paramPropOwner);
    }
    
    // below function takes the inputs PropertyID, PropertyName and 
    // PropertyAddress and creates a property
    function CreateProperty (int256 PropertyID, string memory PropertyName, string memory PropertyAddress) 
    public
    {
        require(properties[PropertyID].PropID !=PropertyID,"Property with the specified ID is already exist");
        
       Property memory prop;
       prop.PropID = PropertyID;
       prop.PropName = PropertyName;
       prop.PropAddress = PropertyAddress;
       prop.owner = msg.sender; 
       
       properties[PropertyID] = prop;
       propCount++;
    }
    
    // the below function gets the details of a property 
    // based on Property ID 
    function getProperty (int256 PropID) public view returns (int256 Prop_ID, string memory Prop_Name, string memory Prop_Address, address Prop_Owner) 
    {
        Prop_ID=-1;
        Prop_Name="";
        Prop_Address="";
        
        
        Property memory prop = properties[PropID];
        
        Prop_ID = prop.PropID;
        Prop_Name = prop.PropName;
        Prop_Address = prop.PropAddress;
        Prop_Owner= prop.owner;
        
        return (Prop_ID, Prop_Name, Prop_Address, Prop_Owner);
    }
    
    // below function transfers a property to a new Owner, 
    // but this must be called by the current Owner of the property
    function Transfer (int256 PropertyID, address newOwner) public returns (bool isOk){
         
         if (properties[PropertyID].PropID == PropertyID )
         {
             require(IsOwner(properties[PropertyID].owner),"Only Owner can Transfer the PROPERTY");
             
             properties[PropertyID].owner = newOwner;
             //console.log("Property has been transferred SUCCESSFULLY");
             return (true);
             
         }
         else
         {
             //console.log("Property transfer FAILED");
             return (false);
         }
    }
    
    // below function gets the count of number of created properties

    function getPropertyCount() view public returns (int256 count) 
    {
        return (propCount);
    }
        
}
