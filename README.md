# Binding SelectionChanged event to Listview

In ListView, the [SelectionChanged](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.SfListView~SelectionChanged_EV.html) event is raised once the selection process has been completed. MVVM for the `SelectionChanged` event can be achieved by binding through the event to command converter.

Refer [EventToCommand](https://www.syncfusion.com/kb/7523/how-to-turn-events-into-commands-with-behaviors-in-sflistview?_ga=2.117039750.1823797169.1563166718-1085055173.1562420655) knowledge base to create the command for event using behavior.

```
<syncfusion:SfListView x:Name="listView" 
                       ItemsSource="{Binding BookInfoCollection}">
    <syncfusion:SfListView.Behaviors>
        <local:EventToCommandBehavior EventName="SelectionChanged" Command="{Binding SelectedItem}" 
                                          Converter="{StaticResource EventArgs}"/>
    </syncfusion:SfListView.Behaviors>
</syncfusion:SfListView>
```
```
//ViewModel.cs
public class BookInfoRepository : INotifyPropertyChanged
{
    private Command<ItemSelectionChangedEventArgs> selectedItem

    public Command<ItemSelectionChangedEventArgs> SelectedItem
    {
        get { return this.selectedItem; }
        set
        {
            this.selectedItem = value;
            this.OnPropertyChanged("SelectedItem");
        }
    }

    public BookInfoRepository()
    {
        selectedItem = new Command<ItemSelectionChangedEventArgs>(OnSelectionChanged);
    }

    ///<summary>
    ///Remove the selected item
    ///</summary>
    public void OnSelectionChanged(ItemSelectionChangedEventArgs obj)
    {
        var eventArgs = obj as ItemSelectionChangedEventArgs;
        var item= eventArgs.AddedItems[0];
        this.bookInfoCollection.Remove(this.BookInfoCollection.FirstOrDefault(x => x == item));
    }
}
```
To know more about MVVM in ListView, please refer our documentation [here](https://help.syncfusion.com/xamarin/sflistview/mvvm)