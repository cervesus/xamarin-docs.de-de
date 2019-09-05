---
ms.assetid: 4D47185C-8998-4903-AE64-7E2A67F9DF7A
title: Vergleichen von Benutzeroberflächen-
description: Dieses Dokument enthält einen Vergleich der UI-Steuerelemente zwischen xamarin. Forms, Windows Forms und WPF. Außerdem wird mit einer anderen Dokumentation verknüpft, die WPF mit xamarin. Forms vergleicht.
author: conceptdev
ms.author: crdun
ms.date: 04/26/2017
ms.openlocfilehash: 7ac1c7872253f11ddd58e362501827058fd9a28a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290393"
---
# <a name="ui-controls-comparison"></a>Vergleichen von Benutzeroberflächen-

Im folgenden finden Sie einen Vergleich der xamarin. Forms-Steuerelemente mit Windows Forms und WPF, die auf [dieser Tabelle](/dotnet/framework/wpf/advanced/windows-forms-controls-and-equivalent-wpf-controls)basieren.

Erfahren Sie mehr über die [Ähnlichkeiten und Unterschiede zwischen WPF und xamarin. Forms](wpf.md) , um Ihre Desktop Kenntnisse für die Mobile App Entwicklung zu aktualisieren.

|Windows Forms|WPF|Xamarin.Forms|
|--- |--- |--- |
|[BindingNavigator](https://msdn.microsoft.com/library/system.windows.forms.bindingnavigator(v=vs.110).aspx)|-|-|
|[BindingSource](https://msdn.microsoft.com/library/system.windows.forms.bindingsource(v=vs.110).aspx)|[CollectionViewSource](https://msdn.microsoft.com/library/system.windows.data.collectionviewsource(v=vs.110).aspx)|Bindungs Eigenschaft, z. b. BindingContext|
|[Button](https://msdn.microsoft.com/library/system.windows.forms.button(v=vs.110).aspx) (Schaltfläche)|[Button](https://msdn.microsoft.com/library/system.windows.controls.button(v=vs.110).aspx) (Schaltfläche)|Schaltfläche|
|[CheckBox](https://msdn.microsoft.com/library/system.windows.forms.checkbox(v=vs.110).aspx)|[CheckBox](https://msdn.microsoft.com/library/system.windows.controls.checkbox(v=vs.110).aspx)|Schalter|
|[CheckedListBox](https://msdn.microsoft.com/library/system.windows.forms.checkedlistbox(v=vs.110).aspx)|[ListBox](https://msdn.microsoft.com/library/system.windows.controls.listbox(v=vs.110).aspx) mit Komposition.|ListView mit Komposition.|
|[ColorDialog](https://msdn.microsoft.com/library/system.windows.forms.colordialog(v=vs.110).aspx)|-|-|
|[ComboBox](https://msdn.microsoft.com/library/system.windows.forms.combobox(v=vs.110).aspx)|Kombinations [Feld](https://msdn.microsoft.com/library/system.windows.controls.combobox(v=vs.110).aspx) (bietet keine Unterstützung für die automatische Vervollständigung)|Auswahl|
|[ContextMenuStrip](https://msdn.microsoft.com/library/system.windows.forms.contextmenustrip(v=vs.110).aspx)|[ContextMenu](https://msdn.microsoft.com/library/system.windows.controls.contextmenu(v=vs.110).aspx)|-|
|[DataGridView](https://msdn.microsoft.com/library/system.windows.forms.datagridview(v=vs.110).aspx)|[DataGrid](https://msdn.microsoft.com/library/system.windows.controls.datagrid(v=vs.110).aspx)|-|
|[DateTimePicker](https://msdn.microsoft.com/library/system.windows.forms.datetimepicker(v=vs.110).aspx)|[DatePicker](https://msdn.microsoft.com/library/system.windows.controls.datepicker(v=vs.110).aspx)|DatePicker-& timePicker|
|[DomainUpDown](https://msdn.microsoft.com/library/system.windows.forms.domainupdown(v=vs.110).aspx)|[Textfeld](https://msdn.microsoft.com/library/system.windows.controls.textbox(v=vs.110).aspx) und zwei [RepeatButton](https://msdn.microsoft.com/library/system.windows.controls.primitives.repeatbutton(v=vs.110).aspx) -Steuerelemente.|Stepper|
|[ErrorProvider](https://msdn.microsoft.com/library/system.windows.forms.errorprovider(v=vs.110).aspx)|-|-|
|[FlowLayoutPanel](https://msdn.microsoft.com/library/system.windows.forms.flowlayoutpanel(v=vs.110).aspx)|[WrapPanel](https://msdn.microsoft.com/library/system.windows.controls.wrappanel(v=vs.110).aspx) oder [StackPanel](https://msdn.microsoft.com/library/system.windows.controls.stackpanel(v=vs.110).aspx)|Stacklayout oder flexlayout|
|[FolderBrowserDialog](https://msdn.microsoft.com/library/system.windows.forms.folderbrowserdialog(v=vs.110).aspx)|-|-|
|[FontDialog](https://msdn.microsoft.com/library/system.windows.forms.fontdialog(v=vs.110).aspx)|-|-|
|[Formular](https://msdn.microsoft.com/library/system.windows.forms.form(v=vs.110).aspx)|[Fenster](https://msdn.microsoft.com/library/system.windows.window(v=vs.110).aspx)|Seite|
|[GroupBox](https://msdn.microsoft.com/library/system.windows.forms.groupbox(v=vs.110).aspx)|[GroupBox](https://msdn.microsoft.com/library/system.windows.controls.groupbox(v=vs.110).aspx)|-|
|[HelpProvider](https://msdn.microsoft.com/library/system.windows.forms.helpprovider(v=vs.110).aspx)|Kein entsprechendes Steuerelement (Quick Infos verwenden).|-|
|[HScrollBar](https://msdn.microsoft.com/library/system.windows.forms.hscrollbar(v=vs.110).aspx)|[Scrollleiste](https://msdn.microsoft.com/library/system.windows.controls.primitives.scrollbar(v=vs.110).aspx) (Scrollen ist in Container Steuerelemente integriert)|Verwenden von ScrollView|
|[ImageList](https://msdn.microsoft.com/library/system.windows.forms.imagelist(v=vs.110).aspx)|-|-|
|[Bezeichnung](https://msdn.microsoft.com/library/system.windows.forms.label(v=vs.110).aspx)|[Bezeichnung](https://msdn.microsoft.com/library/system.windows.controls.label(v=vs.110).aspx)|Bezeichnung|
|[LinkLabel](https://msdn.microsoft.com/library/system.windows.forms.linklabel(v=vs.110).aspx)|Kein entsprechendes Steuerelement (Sie können die [Link](https://msdn.microsoft.com/library/system.windows.documents.hyperlink(v=vs.110).aspx) Klasse verwenden, um Hyperlinks innerhalb von fortlaufendem Inhalt zu hosten).|-|
|[ListBox](https://msdn.microsoft.com/library/system.windows.forms.listbox(v=vs.110).aspx)|[ListBox](https://msdn.microsoft.com/library/system.windows.controls.listbox(v=vs.110).aspx)|Verwenden von ListView|
|[ListView](https://msdn.microsoft.com/library/system.windows.forms.listview(v=vs.110).aspx)|[ListView](https://msdn.microsoft.com/library/system.windows.controls.listview(v=vs.110).aspx)|ListView|
|[MaskedTextBox](https://msdn.microsoft.com/library/system.windows.forms.maskedtextbox(v=vs.110).aspx)|-|-|
|[MenuStrip](https://msdn.microsoft.com/library/system.windows.forms.menustrip(v=vs.110).aspx)|[Menü](https://msdn.microsoft.com/library/system.windows.controls.menu(v=vs.110).aspx)|Masterdetailpage oder tabbedpage in Erwägung gezogen|
|[MonthCalendar](https://msdn.microsoft.com/library/system.windows.forms.monthcalendar(v=vs.110).aspx)|[Kalender](https://msdn.microsoft.com/library/system.windows.controls.calendar(v=vs.110).aspx)|-|
|[NotifyIcon](https://msdn.microsoft.com/library/system.windows.forms.notifyicon(v=vs.110).aspx)|-|-|
|[NumericUpDown](https://msdn.microsoft.com/library/system.windows.forms.numericupdown(v=vs.110).aspx)|[Textfeld](https://msdn.microsoft.com/library/system.windows.controls.textbox(v=vs.110).aspx) und zwei [RepeatButton](https://msdn.microsoft.com/library/system.windows.controls.primitives.repeatbutton(v=vs.110).aspx) -Steuerelemente.|Stepper|
|[OpenFileDialog](https://msdn.microsoft.com/library/system.windows.forms.openfiledialog(v=vs.110).aspx)|[OpenFileDialog](https://msdn.microsoft.com/library/microsoft.win32.openfiledialog(v=vs.110).aspx)|-|
|[PageSetupDialog](https://msdn.microsoft.com/library/system.windows.forms.pagesetupdialog(v=vs.110).aspx)|-|-|
|[Bereich](https://msdn.microsoft.com/library/system.windows.forms.panel(v=vs.110).aspx)|[Canvas](https://msdn.microsoft.com/library/system.windows.controls.canvas(v=vs.110).aspx)|Anzeigen oder AbsoluteLayout|
|[PictureBox](https://msdn.microsoft.com/library/system.windows.forms.picturebox(v=vs.110).aspx)|[Image](https://msdn.microsoft.com/library/system.windows.controls.image(v=vs.110).aspx)|Bild|
|[PrintDialog](https://msdn.microsoft.com/library/system.windows.forms.printdialog(v=vs.110).aspx)|[PrintDialog](https://msdn.microsoft.com/library/system.windows.controls.printdialog(v=vs.110).aspx)|-|
|[PrintDocument](https://msdn.microsoft.com/library/system.drawing.printing.printdocument(v=vs.110).aspx)|-|-|
|[PrintPreviewControl](https://msdn.microsoft.com/library/system.windows.forms.printpreviewcontrol(v=vs.110).aspx)|[DocumentViewer](https://msdn.microsoft.com/library/system.windows.controls.documentviewer(v=vs.110).aspx)|-|
|[PrintPreviewDialog](https://msdn.microsoft.com/library/system.windows.forms.printpreviewdialog(v=vs.110).aspx)|-|-|
|[ProgressBar](https://msdn.microsoft.com/library/system.windows.forms.progressbar(v=vs.110).aspx)|[ProgressBar](https://msdn.microsoft.com/library/system.windows.controls.progressbar(v=vs.110).aspx)|ProgressBar|
|[PropertyGrid](https://msdn.microsoft.com/library/system.windows.forms.propertygrid(v=vs.110).aspx)|-|-|
|[RadioButton](https://msdn.microsoft.com/library/system.windows.forms.radiobutton(v=vs.110).aspx)|[RadioButton](https://msdn.microsoft.com/library/system.windows.controls.radiobutton(v=vs.110).aspx)|-|
|[RichTextBox](https://msdn.microsoft.com/library/system.windows.forms.richtextbox(v=vs.110).aspx)|[RichTextBox](https://msdn.microsoft.com/library/system.windows.controls.richtextbox(v=vs.110).aspx)|Der Editor unterstützt keinen umfangreichen (formatierten) Text, Eintrag für einzeiligen Text.|
|[SaveFileDialog](https://msdn.microsoft.com/library/system.windows.forms.savefiledialog(v=vs.110).aspx)|[SaveFileDialog](https://msdn.microsoft.com/library/microsoft.win32.savefiledialog(v=vs.110).aspx)|-|
|[ScrollableControl](https://msdn.microsoft.com/library/system.windows.forms.scrollablecontrol(v=vs.110).aspx)|[ScrollViewer](https://msdn.microsoft.com/library/system.windows.controls.scrollviewer(v=vs.110).aspx)|ScrollView|
|[SoundPlayer](https://msdn.microsoft.com/library/system.media.soundplayer(v=vs.110).aspx)|[Media Player](https://msdn.microsoft.com/library/system.windows.media.mediaplayer(v=vs.110).aspx)|-|
|[SplitContainer](https://msdn.microsoft.com/library/system.windows.forms.splitcontainer(v=vs.110).aspx)|[GridSplitter](https://msdn.microsoft.com/library/system.windows.controls.gridsplitter(v=vs.110).aspx)|Masterdetailpage in Erwägung gezogen|
|[StatusStrip](https://msdn.microsoft.com/library/system.windows.forms.statusstrip(v=vs.110).aspx)|[StatusBar](https://msdn.microsoft.com/library/system.windows.controls.primitives.statusbar(v=vs.110).aspx)|-|
|[TabControl](https://msdn.microsoft.com/library/system.windows.forms.tabcontrol(v=vs.110).aspx)|[TabControl](https://msdn.microsoft.com/library/system.windows.controls.tabcontrol(v=vs.110).aspx)|TabbedPage|
|[TableLayoutPanel](https://msdn.microsoft.com/library/system.windows.forms.tablelayoutpanel(v=vs.110).aspx)|[Raster](https://msdn.microsoft.com/library/system.windows.controls.grid(v=vs.110).aspx)|Raster|
|[TextBox](https://msdn.microsoft.com/library/system.windows.forms.textbox(v=vs.110).aspx)|[TextBox](https://msdn.microsoft.com/library/system.windows.controls.textbox(v=vs.110).aspx)|Der Editor unterstützt keinen umfangreichen (formatierten) Text.|
|[Zeitgeber](https://msdn.microsoft.com/library/system.windows.forms.timer(v=vs.110).aspx)|[DispatcherTimer](https://msdn.microsoft.com/library/system.windows.threading.dispatchertimer(v=vs.110).aspx)|Device.StartTime()|
|[ToolStrip](https://msdn.microsoft.com/library/system.windows.forms.toolstrip(v=vs.110).aspx)|[ToolBar](https://msdn.microsoft.com/library/system.windows.controls.toolbar(v=vs.110).aspx)|Page. ToolbarItems und ToolBarItem|
|[ToolStripContainer](https://msdn.microsoft.com/library/system.windows.forms.toolstripcontainer(v=vs.110).aspx), [ToolStripDropDown](https://msdn.microsoft.com/library/system.windows.forms.toolstripdropdown(v=vs.110).aspx), [ToolStripDropDownMenu](https://msdn.microsoft.com/library/system.windows.forms.toolstripdropdownmenu(v=vs.110).aspx), [ToolStripPanel](https://msdn.microsoft.com/library/system.windows.forms.toolstrippanel(v=vs.110).aspx)|[Symbolleiste](https://msdn.microsoft.com/library/system.windows.controls.toolbar(v=vs.110).aspx) mit Komposition.|Page. ToolbarItems und ToolBarItem mit Komposition|
|[QuickInfo](https://msdn.microsoft.com/library/system.windows.forms.tooltip(v=vs.110).aspx)|[QuickInfo](https://msdn.microsoft.com/library/system.windows.controls.tooltip(v=vs.110).aspx)|Verwenden von Barrierefreiheits Funktionen|
|[TrackBar](https://msdn.microsoft.com/library/system.windows.forms.trackbar(v=vs.110).aspx)|[Schieberegler](https://msdn.microsoft.com/library/system.windows.controls.slider(v=vs.110).aspx)|Slider|
|[TreeView](https://msdn.microsoft.com/library/system.windows.forms.treeview(v=vs.110).aspx)|[TreeView](https://msdn.microsoft.com/library/system.windows.controls.treeview(v=vs.110).aspx)|Hierarchische ListView in einer navigationpage|
|[UserControl](https://msdn.microsoft.com/library/system.windows.forms.usercontrol(v=vs.110).aspx)|[UserControl](https://msdn.microsoft.com/library/system.windows.controls.usercontrol(v=vs.110).aspx)|Anzeigen und auch benutzerdefinierte Renderer|
|[VScrollBar](https://msdn.microsoft.com/library/system.windows.forms.vscrollbar(v=vs.110).aspx)|[ScrollBar](https://msdn.microsoft.com/library/system.windows.controls.primitives.scrollbar(v=vs.110).aspx)|Verwenden von ScrollView|
|[WebBrowser](https://msdn.microsoft.com/library/system.windows.forms.webbrowser(v=vs.110).aspx)|[WebBrowser](https://msdn.microsoft.com/library/system.windows.controls.webbrowser(v=vs.110).aspx)|WebView|
