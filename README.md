# reate-a-Contact-Count-field-on-Account.-Write-a-trigger-on-Contact-such-that-if-Contact-is-created-
reate a Contact Count field on Account. Write a trigger on Contact such that  if Contact is created then the parent Account should maintain the Count of total related child Contacts.
trigger countContacts1 on Contact (before update,after update)
{
    if((trigger.isafter && trigger.isupdate) && (trigger.isbefore && trigger.isupdate))
    {
        myclasshandlers.mymethod1(trigger.new);
    }
}







public class myclasshandlers
{
    public static void mymethod1(List<Contact> listNewCon)
    {
        set<id> accountsid=new set<id>();
        list<account>myupdatedlist=new list<account>();
        for(Contact Con1:listNewCon)  //frist we hav to contat id are saved in a set
        {
            accountsid.add(Con1.Id);
        }
        
        list<Account>acclist=[Select id,Active_Contacts__c,(Select id From Contacts)From Account where id IN:accountsid];
        
        
       For(account acc1:acclist)
       {
           acc1.Active_Contacts__c =0;
           acc1.Active_Contacts__c=acc1.Contacts.size();
           
           myupdatedlist.add(acc1);
           
       }
        
        if(!myupdatedlist.isEmpty())
        {
            update myupdatedlist;
        }
    }
}

