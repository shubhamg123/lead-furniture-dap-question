# lead-furniture-dap-question

public class DAPquestion1 {
    public static void mymathod(List<lead> lList){
    // get all account fields from database schema
   Schema.SObjectType objectType = lead.getSObjectType(); //Note that you can actually call getSObjectType on the type name without instantiating a variable
    List<String> leadfield = new List<string>(objectType.getDescribe().fields.getMap().keySet());

        //getting records from lead of new list inserted
        string soql = 'select '+ String.join(leadfield, ',') +' from lead where id in :lList';
   
        list<lead> newleads = Database.query(soql);
        system.debug(newleads);

list<id> llistid=new list<id>();
        List<string> newname = new List<string>();
        List<string> newemail = new List<string>();
        List<string> newphone = new List<string>();
        List<string> newzipcode = new List<string>();
        for(lead l:Llist)
        {
            if(l.FirstName!=null && l.Email!=null && l.Phone!=null && l.PostalCode!=null){
            newname.add(l.firstname);
                llistid.add(l.id);
                newemail.add(l.Email);
                newphone.add(l.Phone);
               newzipcode.add(l.PostalCode);
            }
        }
        system.debug(newname);
       
       
       
       
       
        //getting records allready present same as lead of trigger.new
        string soql1 = 'select '+ String.join(leadfield, ',') +' from lead  where firstname in :oldname and id not in :llistid and email in:oldemail and phone in : oldphone and postalcode in : oldzipcode ';// and id in :llist
        List<lead> oldleads = Database.query(soql1);
                system.debug(oldresult);
       
         list<lead> toupdate=new List<lead>();
        list<lead> todelete=new List<lead>();
       

        FOR(lead l:newleads){
            for(lead l1:oldleads){
                if(l.FirstName==l1.firstname&& l.Phone==l1.phone&&l.Email==l1.email &&l.PostalCode==l1.postalcode  ){
                    toupdate.add(l);
                    todelete.add(l1);
                }
            }
        }
        system.debug(todelete);
        system.debug(toupdate);
               String objType='lead';
Map<String, Schema.SObjectType> schemaMap = Schema.getGlobalDescribe();
Schema.SObjectType leadSchema = schemaMap.get(objType);
Map<String, Schema.SObjectField> fieldMap = leadSchema.getDescribe().fields.getMap();

       
        integer size=ms.size();
        for(integer i=0 ;i<size;i++){
            for(string s :leadfield){
               // Schema.SobjectField theField = objectType.getDescribe().fields.getMap().get(s);
                //Schema.DisplayType f = Schema.getGlobalDescribe().getType(thefield);
                Schema.DisplayType fielddataType = fieldMap.get(s).getDescribe().getType();
                system.debug(s+'=>'+string.valueOf(fielddataType));
               

                if(todelete[i].get(s)!=null && toupdate[i].get(s)==null &&s!='address'){
                     string s1=string.valueof(todelete[i].get(s));
                    if(string.valueOf(fielddataType)=='INTEGER'){
                    toupdate[i].put(s,integer.valueOf(s1) );

                    }

                    else if(string.valueOf(fielddataType)=='CURRENCY'){
                        toupdate[i].put(s,decimal.valueOf(s1) );
                    }
                    else{

                    system.debug(s);
                    system.debug(s1);
                    toupdate[i].put(s,s1 );
                    }
                   
                }
            }
        }

        update ms;
        delete dl;
     
    }
}
