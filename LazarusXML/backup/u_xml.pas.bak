unit u_xml;

{$mode objfpc}{$H+}

interface

uses
  Classes, SysUtils, FileUtil, LResources, Forms, Controls, Graphics, Dialogs,
  StdCtrls, ComCtrls, Grids, DBGrids, DbCtrls, ZConnection, ZDataset,
  ZSqlUpdate, DOM, XMLWrite, XMLRead, db;

type

  { TfrmXml }

  TfrmXml = class(TForm)
    btnAbrir: TButton;
    btnLer: TButton;
    add_no: TButton;
    Button2: TButton;
    DataSource1: TDataSource;
    DBGrid1: TDBGrid;
    DBNavigator1: TDBNavigator;
    Edit1: TEdit;
    Memo1: TMemo;
    edit_add: TEdit;
    Label3: TLabel;
    remover_no: TButton;
    edArquivo: TEdit;
    Label1: TLabel;
    dlgAbrir: TOpenDialog;
    Label2: TLabel;
    selecao_no: TEdit;
    tvwXML: TTreeView;
    ZConnection1: TZConnection;
    ZTable1: TZTable;
    procedure add_noClick(Sender: TObject);
    procedure btnAbrirClick(Sender: TObject);
    procedure btnLerClick(Sender: TObject);
    procedure Button1Click(Sender: TObject);
    procedure Button2Click(Sender: TObject);
    procedure DomToTree(XmlNode: TDOMNode; TreeNode: TTreeNode);
    procedure Edit1Change(Sender: TObject);
    procedure selecao_noChange(Sender: TObject);
    procedure FormCreate(Sender: TObject);
    procedure Label2Click(Sender: TObject);
    procedure remover_noClick(Sender: TObject);
  private
    { private declarations }
  public
    { public declarations }
    Documento: TXMLDocument;
  end;

var
  frmXml: TfrmXml;

implementation

{$R *.lfm}

{ TfrmXml }

procedure TfrmXml.btnAbrirClick(Sender: TObject);
begin
  if dlgAbrir.Execute then
  begin
    edArquivo.Text := dlgAbrir.FileName;
    btnLer.Enabled := True;
  end;
end;

procedure TfrmXml.add_noClick(Sender: TObject);
var
  i: integer;
  s: string;
begin
  // Se não tem nom cria um no sem filhos
  if tvwXML.Items.Count = 0 then
  begin
    tvwXML.Items.Add (nil,edit_add.Text);
    exit;
  end;

  // Set up a simple text for each new node - Node1 , Node2 etc
  i := tvwXML.Items.Count;
  s := edit_add.Text;
  //Add um no ao no selecionado
  if tvwXML.Selected <> nil then
    tvwXML.Items.AddChild(tvwXML.Selected ,s);
end;

procedure TfrmXml.btnLerClick(Sender: TObject);
begin
  tvwXML.Items.Clear;
  ReadXMLFile(Documento, edArquivo.Text);
  DomToTree(Documento.DocumentElement, nil);
  tvwXML.FullExpand;
end;

procedure TfrmXml.Button1Click(Sender: TObject);
begin

end;

procedure TfrmXml.Button2Click(Sender: TObject);
var
  termo: String;
  xml_nome: TDOMNode;
  xmlnode: TDOMNode;
  xmlroot: TDOMNode;
  xml_email: TDOMNode;
  i: integer;
begin
  xmlroot := Documento.DocumentElement;
  for i := 0 to xmlroot.ChildNodes.Count - 1 do
  begin
    xmlnode := xmlroot.ChildNodes.Item[i];
    xml_nome := xmlnode.FindNode(Edit1.Text);
    //xml_email := xmlnode.FindNode('email');
    if xml_nome <> nil then
    begin
      Memo1.Lines.Add('- ' + xml_nome.TextContent);
      termo := xml_nome.TextContent;
      ZTable1.Append;
      ZTable1.FieldByName('termos').AsString:= termo;
      Ztable1.Post;
    end;
  end;
end;

procedure TfrmXml.DomToTree(XmlNode: TDOMNode; TreeNode: TTreeNode);
var
  I: integer;
  NewTreeNode: TTreeNode;
  NodeText: string;
  AttrNode: TDOMNode;
begin
  // considera apenas elementos
  if XmlNode.NodeType <> ELEMENT_NODE then
    Exit;
  // inclui o nó
  NodeText := XmlNode.NodeName;
  if (XmlNode.ChildNodes.Count = 1) and (XmlNode.ChildNodes[0].NodeValue <> '')  then
    NodeText := NodeText + ' = ' + XmlNode.ChildNodes[0].NodeValue;
  NewTreeNode := tvwXML.Items.AddChild(TreeNode, NodeText);
  // inclui atributos
  for I := 0 to xmlNode.Attributes.Length - 1 do
  begin
    AttrNode := xmlNode.Attributes.Item[I];
    tvwXML.Items.AddChild(NewTreeNode,
    '[' + AttrNode.NodeName + ' = "' + AttrNode.NodeValue + '"]');
  end;
  // inclui cada nó filho
  if XmlNode.HasChildNodes then
    for I := 0 to xmlNode.ChildNodes.Count - 1 do
    DomToTree(xmlNode.ChildNodes.Item[I], NewTreeNode);
end;

procedure TfrmXml.Edit1Change(Sender: TObject);
begin

end;

procedure TfrmXml.selecao_noChange(Sender: TObject);
begin

end;

procedure TfrmXml.FormCreate(Sender: TObject);
begin

end;

procedure TfrmXml.Label2Click(Sender: TObject);
begin

end;

procedure TfrmXml.remover_noClick(Sender: TObject);
//Procedimento recursivo para deletar no
  procedure DeleteNode(Node:TTreeNode);
  begin
    while Node.HasChildren do
      DeleteNode(node.GetLastChild);
    tvwXML.Items.Delete(Node) ;
  end;

begin
  if tvwXML.Selected = nil then
    exit;

    //if selected node has child nodes, first ask for confirmation
  if tvwXML.Selected.HasChildren then
    if messagedlg( 'Delete node and all children ?',mtConfirmation, [mbYes,mbNo],0 ) <> mrYes then
      exit;
  DeleteNode(tvwXML.Selected);
end;


end.

