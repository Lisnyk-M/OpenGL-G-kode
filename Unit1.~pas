unit Unit1;

interface

uses
  Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms,
  Dialogs, StdCtrls, OpenGL, ExtCtrls, Menus;

type
  Vector = record
    x, y, z : GLfloat;
  end;



type
  TForm1 = class(TForm)
    Button1: TButton;
    Panel1: TPanel;
    Edit1: TEdit;
    Edit2: TEdit;
    Edit3: TEdit;
    Edit4: TEdit;
    Edit5: TEdit;
    Edit6: TEdit;
    Label1: TLabel;
    Label2: TLabel;
    Label3: TLabel;
    Label4: TLabel;
    Label5: TLabel;
    Label6: TLabel;
    Button2: TButton;
    Button3: TButton;
    Button4: TButton;
    Button5: TButton;
    Button6: TButton;
    Button7: TButton;
    Button8: TButton;
    Button9: TButton;
    MainMenu1: TMainMenu;
    File1: TMenuItem;
    Button10: TButton;
    N1: TMenuItem;
    N2: TMenuItem;
    G1: TMenuItem;
    Button11: TButton;
    function SelectFiles():string;
    procedure Button1Click(Sender: TObject);
    procedure FormCreate(Sender: TObject);
    procedure FormDestroy(Sender: TObject);
    procedure FormKeyDown(Sender: TObject; var Key: Word;
      Shift: TShiftState);
    procedure FormMouseMove(Sender: TObject; Shift: TShiftState; X,
      Y: Integer);
    procedure FormMouseUp(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    procedure FormMouseDown(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    procedure FormResize(Sender: TObject);
    procedure FormClose(Sender: TObject; var Action: TCloseAction);
    procedure File1Click(Sender: TObject);
    procedure N2Click(Sender: TObject);
    procedure G1Click(Sender: TObject);
    procedure Button2Click(Sender: TObject);
    procedure Button3Click(Sender: TObject);
    procedure Button4Click(Sender: TObject);
    procedure Button5Click(Sender: TObject);
    procedure Button6Click(Sender: TObject);
    procedure Button7Click(Sender: TObject);
    procedure Button8Click(Sender: TObject);
    procedure Button9Click(Sender: TObject);
    procedure Button10Click(Sender: TObject);
    procedure Edit2Change(Sender: TObject);
    procedure Edit1Change(Sender: TObject);
    procedure Button11Click(Sender: TObject);
  private
    { Private declarations }
    DC: HDC;
    hrc: HGLRC;
    Model, Normals : TList;
    lastx, down : Integer;
    procedure generate3D;
    procedure Init;
    procedure CalcNormals;
    procedure CalcNormals1(dv1, dv2, dv3 : Vector; var nrm : Vector);
    procedure GenGkode (FileName : string; strHeader : string; strEnd : string; Depth, FRZwidth : GLfloat; Frouge, Ffinish, Fdepth : GLfloat);
    procedure CreateGLlist;
    procedure SetDCPixelFormat;
    procedure PrepareImage;
    // ????????? ???????? ?????? ????
procedure MouseWheel(Sender: TObject; Shift: TShiftState;
WheelDelta: Integer; MousePos: TPoint; var Handled: Boolean);
procedure MouseWheelDown(Sender: TObject; Shift: TShiftState;
MousePos: TPoint; var Handled: Boolean);


// ????????? ????????? ?????? ?????
procedure MouseWheelUp(Sender: TObject; Shift: TShiftState;
MousePos: TPoint; var Handled: Boolean);


  //  procedure LoadDXF (st : String);
  protected
    procedure WMPaint(var Msg: TWMPaint); message WM_PAINT;

  public
    { Public declarations }
  end;
const
  SURFACE = 1;
  nSec = 20;

var
  Form1: TForm1;
  Fnew : TextFile;
  openDialog : TOpenDialog;    // Переменная OpenDialog
  s, st, t, ssX, ssY, ssZ : string;
  n, vx, vy, vz :extended;
  count : integer;
   Model, Model3D , Normals, VertexArray : TList;
//   const
//  SURFACE = 1;

  ang   : GLfloat = 0;
  ang1  : GLfloat = 0;
  delta : GLfloat = 0;
  Angle : GLfloat = 0;
  Step : GLfloat = 1;
  wrkStep : GLfloat = 1;
  wrkTime : longint;
  wrkX, wrkY : Integer;
  //fs: TFormatSettings;
  v1, v2, v3, v4 : Vector;
  scFactor : GLfloat;
  distToCentr : GLfloat;
  trig : boolean;   //вимикає генерацію подачі для всіх строк чорнового коду крім першої

  DepthZahodiv, Fzahodiv : GLfloat;
  Br, Xgl, Ygl, Zgl : GLfloat;
  Mode : integer;
  Xm : GLfloat;
  FileNm : string;

implementation

{$R *.dfm} {$R Moy.res}
function TForm1.SelectFiles():string;
var
  pv1_s : ^Vector;
  v1_s : Vector;
  k_s : integer;

procedure AddToList (x, y, z : GLfloat);
var
  wrkVector : Vector;
  pwrkVector : ^Vector;
begin
  wrkVector.x := x;
  wrkVector.y := y;
  wrkVector.z := z;
  New (pwrkVector);
  pwrkVector^ := wrkVector;
  VertexArray.Add (pwrkVector);
end;
begin
//  assignFile(FZnaity,'' );
 //  reset(FZnaity);
 //  closeFile (Fnew);
   // Создание объекта OpenDialog - назначение на нашу переменную OpenDialog
  openDialog := TOpenDialog.Create(self);

  // Установка начального каталога, чтобы сделать его текущим
  openDialog.InitialDir := GetCurrentDir;

  // Только разрешенные существующие файлы могут быть выбраны
  openDialog.Options := [ofFileMustExist];

  // Разрешено выбрать только .dpr и .pas файлы
  openDialog.Filter :=
    'Stp files|*.stp';

  // Выбор файлов Паскаля как стартовый тип фильтра
  openDialog.FilterIndex := 2;

  // Показ диалог открытия файла
 if openDialog.Execute
  then ShowMessage('File : '+openDialog.FileName)
  else ShowMessage('Открытие файла остановлено');

   FileNm := openDialog.FileName;
   assignFile(Fnew, openDialog.FileName );
   reset(Fnew);
   while (not eof(Fnew)) do
   begin
     readln (Fnew, s);
     //Якщо pos<>0 то знайдено строку яка містить підстроку
     if pos('CARTESIAN_POINT($,(0.,0.,0.));', s) <> 0 then
     begin
//       repeat
     while  pos('CARTESIAN_POINT($,(0.,0.,0.));', st) = 0 do
     begin
       readln (Fnew, st);
       if pos('CARTESIAN_POINT($,(0.,0.,0.));', st) <> 0 then break;
       ssX := '';
       ssY := '';
       ssZ := '';
       t:= copy (st, pos ('CARTESIAN_POINT($,(', st ) + length ('CARTESIAN_POINT($,(' ), length (st)-2);
       count := 1;
       repeat
         ssX:= ssX+t[count];
         count:= count+1;
       until t[count]=',';
       count:= count+1;
       repeat
         ssY:= ssY+t[count];
         count:= count+1;
       until t[count]=',';
       count:= count+1;
       repeat
         ssZ:= ssZ+t[count];
         count:= count+1;
       until t[count]=')';
   {    if ssX[length(ssX)] = '.' then
         ssx := copy (ssx, 0, length(ssX) - 1);

       if ssY[length(ssY)] = '.' then
         ssY := copy (ssY, 0, length(ssY) - 1);

       if ssZ[length(ssZ)] = '.' then
         ssZ := copy (ssZ, 0, length(ssZ) - 1);}
     //  GetLocaleFormatSettings(1,fs);
     decimalseparator := '.';
       vx := strtofloat(ssX);
       vy := strtofloat(ssY);
       vz := strtofloat(ssZ);
       AddToList (vx, vy, vz);
      // until pos('CARTESIAN_POINT($,(0.,0.,0.));', st) <> 0;
      end; //while do
     end;
   end;
   closeFile (Fnew);
   //GenGkode ('bolgarka.txt', 'G0,G1,G55,F1060', 'G0,G1',25, 2, 1060, 1060);
  // Освобождение диалога
  openDialog.Free;
//======= Знаходимо максимальний Х ===========
  Xm := 0;
  for k_s := 0 to VertexArray.Count -1 do    //
  begin                                      //
      pv1_s := VertexArray[k_s];             //
      v1_s := pv1_s^;                        //
      if v1_s.x > Xm then                  //
          Xm := v1_s.x;                    //
  end;                                       //
//============================================

end;
{$HINTS OFF}
//===============================================================
procedure TForm1.CalcNormals1(dv1, dv2, dv3 : Vector; var nrm : Vector);
var
  i : Integer;
  wrki, vx1, vy1, vz1, vx2, vy2, vz2 : GLfloat;
  nx, ny, nz : GLfloat;
  wrkVector : Vector;
  pwrkVector : ^Vector;
  wrkVector1, wrkVector2, wrkVector3 : Vector;
  pwrkVector1, pwrkVector2, pwrkVector3 : ^Vector;
  pnrm : ^Vector;
begin
//  New (pwrkVector1);
//  New (pwrkVector2);
//  New (pwrkVector3);

 // For i := 0 to round (Model.Count / 3) - 1 do begin
 {    pwrkVector1 := Model [i * 3];
     wrkVector1 := pwrkVector1^;
     pwrkVector2 := Model [i * 3 + 1];
     wrkVector2 := pwrkVector2^;
     pwrkVector3 := Model [i * 3 + 2];
     wrkVector3 := pwrkVector3^;
  }
     vx1 := dv1.x - dv2.x;
     vy1 := dv1.y - dv2.y;
     vz1 := dv1.z - dv2.z;

     vx2 := dv2.x - dv3.x;
     vy2 := dv2.y - dv3.y;
     vz2 := dv2.z - dv3.z;

     // вектор перпендикулярен центру треугольника
     nx := vy1 * vz2 - vz1 * vy2;
     ny := vz1 * vx2 - vx1 * vz2;
     nz := vx1 * vy2 - vy1 * vx2;

     // получаем унитарный вектор единичной длины
     wrki := sqrt (nx * nx + ny * ny + nz * nz);
     If wrki = 0 then wrki := 1; // для предотвращения деления на ноль

     wrkVector.x := nx / wrki;
     wrkVector.y := ny / wrki;
     wrkVector.z := nz / wrki;

     New (pwrkVector);
     pwrkVector^ := wrkVector;

     Normals.Add (pwrkVector);

     New (pnrm);
     pnrm^ := nrm;
     pnrm^ := wrkVector;
     nrm := wrkVector;
 // end;
end;

//===============================================================
procedure TForm1.CalcNormals;
var
  i : Integer;
  wrki, vx1, vy1, vz1, vx2, vy2, vz2 : GLfloat;
  nx, ny, nz : GLfloat;
  wrkVector : Vector;
  pwrkVector : ^Vector;
  wrkVector1, wrkVector2, wrkVector3 : Vector;
  pwrkVector1, pwrkVector2, pwrkVector3 : ^Vector;
begin
  New (pwrkVector1);
  New (pwrkVector2);
  New (pwrkVector3);

  For i := 0 to round (Model.Count / 3) - 1 do begin
     pwrkVector1 := Model [i * 3];
     wrkVector1 := pwrkVector1^;
     pwrkVector2 := Model [i * 3 + 1];
     wrkVector2 := pwrkVector2^;
     pwrkVector3 := Model [i * 3 + 2];
     wrkVector3 := pwrkVector3^;

     vx1 := wrkVector1.x - wrkVector2.x;
     vy1 := wrkVector1.y - wrkVector2.y;
     vz1 := wrkVector1.z - wrkVector2.z;

     vx2 := wrkVector2.x - wrkVector3.x;
     vy2 := wrkVector2.y - wrkVector3.y;
     vz2 := wrkVector2.z - wrkVector3.z;

     // вектор перпендикулярен центру треугольника
     nx := vy1 * vz2 - vz1 * vy2;
     ny := vz1 * vx2 - vx1 * vz2;
     nz := vx1 * vy2 - vy1 * vx2;

     // получаем унитарный вектор единичной длины
     wrki := sqrt (nx * nx + ny * ny + nz * nz);
     If wrki = 0 then wrki := 1; // для предотвращения деления на ноль

     wrkVector.x := nx / wrki;
     wrkVector.y := ny / wrki;
     wrkVector.z := nz / wrki;

     New (pwrkVector);
     pwrkVector^ := wrkVector;

     Normals.Add (pwrkVector);
  end;
end;
{$HINTS ON}

Procedure TForm1.generate3D;
begin
end;
{=======================================================================
Инициализация}
procedure TForm1.Init;
begin
 glEnable(GL_DEPTH_TEST);
 glEnable(GL_LIGHTING);
 glEnable(GL_LIGHT0);
 glEnable (GL_COLOR_MATERIAL);
 glLightModeli(GL_LIGHT_MODEL_TWO_SIDE, 1);
// glColor3f (0.5, 0.5, 0.5);
 glColor3f (1, 1, 1);
end;

{=======================================================================
Перерисовка окна}
procedure TForm1.WMPaint(var Msg: TWMPaint);
var
  ps : TPaintStruct;
begin
  Form1.Panel1.Repaint;
  Form1.Button1.Repaint;
  BeginPaint (Handle, ps);

  glClear (GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);

  glPushMatrix;
  glScalef (0.2 * scFactor, 0.2 * scFactor, 0.2 * scFactor);
  glColor3f (Br, Br, Br);
  glTranslatef (Xgl, Ygl, Zgl);
  glTranslatef (-Xm /2 , 0, -Xm/2);
 // glRotatef (Angle, 1.0, 0.0, 1.0);
  glRotatef (1 * Angle, 1.0, 0.0, 0.0);

  glCallList (SURFACE);

  glPopMatrix;

  SwapBuffers (DC);
  EndPaint (Handle, ps);

  Angle := Angle + Step;
  If Angle >= 360.0 then Angle := 0.0;
  InvalidateRect(Handle, nil, False);
end;


procedure TForm1.Button1Click(Sender: TObject);
begin
  Form1.Panel1.Repaint;
 // Model := TList.Create;
//  Normals := TList.Create;
//  VertexArray := TList.Create;
  SelectFiles();
  CreateGLlist;
end;
procedure TForm1.CreateGLlist;
var
  ig, kg : Integer;
  pV1, pV2, pV3, pV4 : ^Vector;
  tV1, tV2 : Vector;
  ptV1, ptV2 : ^Vector;
  tNrm : Vector;
  n1, n2, n3, n4 : Vector;
  radius_scale : GLfloat;
  axis_rev : GLfloat;
begin
  //radius := 0.5;
  radius_scale := 1;
  axis_rev := StrTofloat (Form1.Edit2.Text);    //відстань від Y нуля до центру 35mm
  new(ptV1);
  new(ptV2);
  glNewList (SURFACE, GL_COMPILE);
   For ig := 0 to {round }(VertexArray.Count) - 2 do begin
     ang := 0;
     ang1 := (360/nSec) * pi /180;
     ptV1 := VertexArray[Ig];
     tv1 := ptV1^;
     ptV2 := VertexArray[Ig + 1];
     tv2 := ptV2^;

     for kg := 0 to nSec do begin
       v1.z := radius_scale * (axis_rev - tv1.y) * sin (ang);
       v1.y := radius_scale * (axis_rev - tv1.y) * cos (ang);
       v1.x := tv1.x;

       v2.z := radius_scale * (axis_rev - tv2.y) * sin (ang);
       v2.y := radius_scale * (axis_rev - tv2.y) * cos (ang);
       v2.x := tv2.x;
         v3.z := radius_scale * (axis_rev - tv2.y) * sin (ang1);
         v3.y := radius_scale * (axis_rev - tv2.y) * cos (ang1);
         v3.x := tv2.x;
       v4.z := radius_scale * (axis_rev - tv1.y) * sin (ang1);
       v4.y := radius_scale * (axis_rev - tv1.y) * cos (ang1);
       v4.x := tv1.x;

       n1.z := sin(ang);
       n1.y := cos (ang);
       n1.x := 1;

       n2.z := sin(ang);
       n2.y := cos (ang);
       n2.x := 1;

       n3.z := sin(ang1);
       n3.y := cos (ang1);
       n3.x := 1;

       n4.z := sin(ang1);
       n4.y := cos (ang1);
       n4.x := 1;



       ang := kg * (360/nSec) * pi/180;
       ang1 := (kg + 1)*(360/nSec) * pi/180;



       if Mode < 0 then glPolygonMode (GL_FRONT_AND_BACK, GL_LINE)
         else glPolygonMode (GL_FRONT_AND_BACK, GL_FILL);
    //   pV1^ := V1;
       glEnable(GL_TEXTURE_2D);
       calcNormals1 (v1, v3, v2, tNrm);
       glBegin(GL_QUADS);
       //  glNormal3f(tNrm.x, tNrm.y, tNrm.z);
          glNormal3f(n1.x, n1.y, n1.z);
          glTexCoord2f(1/nSec*kg, 1/Xm*v1.x);
         glvertex3f(v1.x, v1.y, v1.z);     //1

          glNormal3f(n4.x, n4.y, n4.z);
          glTexCoord2f(1/nSec*(kg + 1), 1/Xm*v1.x);
         glvertex3f(v4.x, v4.y, v4.z);     //4

          glNormal3f(n3.x, n3.y, n3.z);
          glTexCoord2f(1/nSec*(kg + 1), 1/Xm*(v2.x));
         glvertex3f(v3.x, v3.y, v3.z);     //3

          glNormal3f(n2.x, n2.y, n2.z);
          glTexCoord2f(1/nSec*kg, 1/Xm*(v2.x));
         glvertex3f(v2.x, v2.y, v2.z);     //2


       glEnd;
       glDisable(GL_TEXTURE_2D);
     end;
   end;
  glEndList;

end;
{function GetY (Xw, lastXw, FXw, Yw, lastYw, nYw, nXw : GLfloat;) : GLfloat;
begin
  Result := (Xw - Xlastw)(Xw - nXw)/(LastYw - Yw) + Yw
end;}
procedure TForm1.GenGkode (FileName : string; strHeader : string; strEnd : string; Depth, FRZwidth : GLfloat; Frouge, Ffinish, Fdepth : GLfloat);
var
   X, lastX, FX, Y, lastY, nY, nX : GLfloat;
   Xmax : GLfloat;
   F : TextFile;
   k_g, n_g : integer;
   v1_g, v2_g : Vector;
   pv1_g, pv2_g : ^Vector;
begin
  AssignFile(F, FileName);
  {if FileExists('bolgarka.txt') then} Rewrite(F);
  FX := 1;
  writeLn (F, '(            G-код Згенеровано генератором G-кодів )');
  writeLn (F, '(Автор програми Лісник М.П тел. 0970770816. Email HCLO5@rambler.ru)');
  writeLn (F, 'G00 G49 G40.1 G17 G80 G50 G90');
  writeLn (F, 'G21');
  writeLn(F, '(Rouge)');    //writeLn('');
  writeLn(F, 'M6 T1');
  writeLn(F, 'M03 S1000');
 // writeLn(F, 'G01');


  writeLn (F, 'G01 x' + Format ('%4f', [ -1.0 ]) + ' y' + format ('%4f', [ Depth ]) + ' Z0.0' + ' F' + format ('%4f', [ Fdepth ]));
  writeLn (F, 'Y0.0');

  Xmax := 0;
//======= Знаходимо максимальний Х ===========
  for k_g := 0 to VertexArray.Count -1 do    //
  begin                                      //
      pv1_g := VertexArray[k_g];             //
      v1_g := pv1_g^;                        //
      if v1_g.x > Xmax then                  //
          Xmax := v1_g.x;                    //
  end;                                       //
//============================================




  trig := true;
  while FX <= Xmax do
  begin //1
    for k_g := 0 to VertexArray.Count -2 do
    begin //2
      pv1_g := VertexArray[k_g + 1];
      v1_g := pv1_g^;
      pv2_g := VertexArray[k_g];
      v2_g := pv2_g^;
      if  FX < v1_g.x then
      begin //3
        X := v1_g.x;
        lastX := v2_g.x;
        Y := v1_g.y;
        lastY := v2_g.y;
        nX := FX;
        nY :=  (lastY - Y)*(X - nX)/(X - LastX) + Y;
        if trig = true then begin
          writeLn (F, 'x' + Format( '%4f', [ nX ] ) + ' F' + Format( '%4f', [ Frouge ] ));
          trig := false;
        end
        else
          writeLn (F, 'x' + Format( '%4f', [ nX ] ) );
        writeLn (F, 'y' + Format( '%4f', [ nY ] ) );
        writeLn (F, 'y0.0');
        FX := FX + FRZwidth;
        break;
      end; //3
    end; //2
  end; //1
  writeLn (F, 'x' + Format ('%4f', [ Xmax ]));
  writeLn (F, 'y' + format ('%4f', [ Depth ]) + ' F' + format ('%4f', [ Fdepth ]));
  //=========== Фінішний код =============
  WriteLn (F, '(Finish Maty by yogo za nogu)');
  for k_g := VertexArray.Count-1 downto 0 do
  begin
      pv1_g := VertexArray[k_g];
      v1_g := pv1_g^;
      if k_g = VertexArray.Count-1 then   //перша строка фінішного коду добавляється F60
        writeLn (F, 'x' + Format( '%4f', [ v1_g.x ] ) + ' y' + Format( '%4f', [ v1_g.y + 0.5 ] ) + ' F' + Format( '%4f', [ Ffinish ] ))
      else    // для всіх інших подача не додається
        writeLn (F, 'x' + Format( '%4f', [ v1_g.x ] ) + ' y' + Format( '%4f', [ v1_g.y + 0.5 ] ));
  end;
  writeLn (F, 'G00 Z1');
  writeLn (F, 'M5 M9');
  writeLn (F, 'M30');
  CloseFile(F);
  showMessage ('G-код згенеровано. Дивись у папку !');
end;
procedure TForm1.FormCreate(Sender: TObject);

begin
  Mode := 1;
  Xgl := 0;
  Ygl := 0;
  Zgl := 0;
  Br := 0.15;
  Model := TList.Create;
  Normals := TList.Create;
  VertexArray := TList.Create;
  Form1.Panel1.Repaint;
  Form1.OnMouseWheel:=MouseWheel;
  Form1.OnMouseWheelDown:=MouseWheelDown;

// ????????? ?????? ?????
  Form1.OnMouseWheelUp:=MouseWheelUp;
  scFactor := 1;
  DC := GetDC(Handle);
  SetDCPixelFormat;
  hrc := wglCreateContext(DC);
  wglMakeCurrent(DC, hrc);
  //glClearColor (1.0, 1.0, 1.0, 1.0);
  glClearColor (0.0, 0.0, 1.0, 1.0);
  down := 0;
  Init;
  PrepareImage;

  glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST);
  glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);
  glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);

  //Model := TList.Create;
  //Normals := TList.Create;
//  LoadDxf ('Dolphin.dxf');
 // CalcNormals;
 { glNewList (SURFACE, GL_COMPILE);
   For i := 0 to round (VertexArray.Count) - 1 do begin
     ang := 0;
     ang1 := (360/nSec) * pi /180;
     for k := 0 to nSec do begin
     new(pV1);
     new(pV2);
   //  new(pV3);
  //   new(pV4);
     pV1 := VertexArray[I];
     v1 := pV1^;
     pV2 := VertexArray[I + 1];
     v2 := pV2^;


       v1.z := radius * v1.z * sin (ang);
       v1.y := radius * v1.y * cos (ang);
//       v1.x := v1.x;

       v2.z := radius * v2.z * sin (ang);
       v2.y := radius * v2.y * cos (ang);
         v3.z := radius * v2.z * sin (ang1);
         v3.y := radius * v2.y * cos (ang1);
       v4.z := radius * v1.z * sin (ang1);
       v4.y := radius * v1.y * cos (ang1);

       pV1^ := V1;
       calcNormals1 (v1, v2, v3, tNrm);
       glBegin(GL_QUADS);
         glNormal3f(tNrm.x, tNrm.y, tNrm.z);
         glvertex3f(v1.x, v1.y, v1.z);
         glvertex3f(v2.x, v2.y, v2.z);
         glvertex3f(v3.x, v3.y, v3.z);
         glvertex3f(v4.x, v4.y, v3.z);
       glEnd;
       ang := k * (360/nSec) * pi/180;
       ang1 := (k * (360/nSec) + 1) * pi/180;
     end;
   end;
  glEndList; }
 // Model.Free;
 // Normals.Free;
end;
{=======================================================================
Изменение размеров окна}


procedure TForm1.FormDestroy(Sender: TObject);
begin
 glDeleteLists (SURFACE, 1);
  VertexArray.Free;
  Model.Free;
  Normals.Free;
 wglMakeCurrent(0, 0);
 wglDeleteContext(hrc);
 ReleaseDC(Handle, DC);
 DeleteDC (DC);
end;

procedure TForm1.FormKeyDown(Sender: TObject; var Key: Word;
  Shift: TShiftState);
begin
  If Key = VK_ESCAPE then Close;
  If Key = VK_SPACE then begin
     If step = 0
        then step := wrkStep
        else begin
        wrkStep := step;
        step := 0;
        end
  end;
end;

procedure TForm1.FormMouseMove(Sender: TObject; Shift: TShiftState; X,
  Y: Integer);
begin
  If down <> 0 then begin
     glRotatef (lastx - x, 1, 0, 1);
     lastx := x;
     step := sqrt (sqr(X - wrkX) + sqr (Y - wrkY)) / (GetTickCount - wrkTime + 1);
     InvalidateRect(Handle, nil, False);
  end;
end;

procedure TForm1.FormMouseUp(Sender: TObject; Button: TMouseButton;
  Shift: TShiftState; X, Y: Integer);
begin
 down := 0;
end;

procedure TForm1.FormMouseDown(Sender: TObject; Button: TMouseButton;
  Shift: TShiftState; X, Y: Integer);
begin
 If button = mbLeft then begin
    lastx := X;
    wrkX := X;
    wrkY := Y;
    down := 1;
    wrkTime := GetTickCount;
 end;
end;
{======================================================================
Подготовка текстуры}
procedure TForm1.PrepareImage;
type
  PPixelArray = ^TPixelArray;
  TPixelArray = array [0..0] of Byte;
var
  Bitmap : TBitmap;
  Data : PPixelArray;
  BMInfo : TBitmapInfo;
  I, ImageSize : Integer;
  Temp : Byte;
  MemDC : HDC;
begin
  Bitmap := TBitmap.Create;
 // Bitmap.LoadFromFile ('ф.bmp');
 Bitmap.LoadFromResourceName(hinstance, 'BMP1');
  with BMinfo.bmiHeader do begin
    FillChar (BMInfo, SizeOf(BMInfo), 0);
    biSize := sizeof (TBitmapInfoHeader);
    biBitCount := 24;
    biWidth := Bitmap.Width;
    biHeight := Bitmap.Height;
    ImageSize := biWidth * biHeight;
    biPlanes := 1;
    biCompression := BI_RGB;

    MemDC := CreateCompatibleDC (0);
    GetMem (Data, ImageSize * 3);
    try
      GetDIBits (MemDC, Bitmap.Handle, 0, biHeight, Data, BMInfo, DIB_RGB_COLORS);
      For I := 0 to ImageSize - 1 do begin
          Temp := Data [I * 3];
          Data [I * 3] := Data [I * 3 + 2];
          Data [I * 3 + 2] := Temp;
      end;
      glTexImage2d(GL_TEXTURE_2D, 0, 3, biWidth,
                   biHeight, 0, GL_RGB, GL_UNSIGNED_BYTE, Data);
     finally
      FreeMem (Data);
      DeleteDC (MemDC);
      Bitmap.Free;
   end;
  end;
end;

{======================================================================

{=======================================================================
Устанавливаем формат пикселей}
procedure TForm1.SetDCPixelFormat;
var
  nPixelFormat: Integer;
  pfd: TPixelFormatDescriptor;
begin
  FillChar(pfd, SizeOf(pfd), 0);

  pfd.dwFlags := PFD_DRAW_TO_WINDOW or PFD_SUPPORT_OPENGL or
                 PFD_DOUBLEBUFFER;
  nPixelFormat := ChoosePixelFormat(DC, @pfd);
  SetPixelFormat(DC, nPixelFormat, @pfd);
end;
procedure TForm1.FormResize(Sender: TObject);
begin
 glViewPort (0, 0, ClientWidth, ClientHeight);
 glMatrixMode(GL_PROJECTION);
 glLoadIdentity;
 gluPerspective(40.0, ClientWidth / ClientHeight, 3.0, 143.0);
 glMatrixMode(GL_MODELVIEW);
 glLoadIdentity;
 glTranslatef(0.0, 0.0, -12.0);
 InvalidateRect(Handle, nil, False);
 Form1.Panel1.Repaint;
end;
procedure TForm1.MouseWheel(Sender: TObject; Shift: TShiftState;
WheelDelta: Integer; MousePos: TPoint; var Handled: Boolean);
begin

// ????????? ??????? ?? ???????? ?????? ????
//ShowMessage('?????????? ???????? ?????? ????...');
end;
procedure TForm1.MouseWheelDown(Sender: TObject; Shift: TShiftState;
MousePos: TPoint; var Handled: Boolean);
begin

// ???????? ???????? ?????? ????
//ShowMessage('down');
  scFactor := scFactor - 0.01;
  //Zgl := Zgl - 2;
end;

procedure TForm1.MouseWheelUp(Sender: TObject; Shift: TShiftState;
MousePos: TPoint; var Handled: Boolean);
begin

// ???????? ???????? ?????? ?????
//ShowMessage('up');
    scFactor := scFactor + 0.01;
    //Zgl := Zgl + 2;
end;

procedure TForm1.FormClose(Sender: TObject; var Action: TCloseAction);
begin
  Form1.OnMouseWheel:=nil;
  Form1.OnMouseWheelDown:=nil;
  Form1.OnMouseWheelUp:=nil;
end;

procedure TForm1.File1Click(Sender: TObject);
begin
  showMessage ('Автор програми Лісник М.П. тел 0970770816');
end;

procedure TForm1.N2Click(Sender: TObject);
begin
  ShowMessage ('Для конвертації кривої Rhino в stp файл необхідно :1.вибрати криву 2.вибрати меню curve/convert/curve to lines 3.експортувати утворену криву в stp файл ');
end;

procedure TForm1.G1Click(Sender: TObject);
begin
  showMessage ('G-код створюється в папці програми з іменем відкритого файла з розширенням .nc');
end;

procedure TForm1.Button2Click(Sender: TObject);
begin
  Xgl := Xgl + 5;
end;

procedure TForm1.Button3Click(Sender: TObject);
begin
  Xgl := Xgl - 5;
end;

procedure TForm1.Button4Click(Sender: TObject);
begin
 Ygl := Ygl + 5;
end;

procedure TForm1.Button5Click(Sender: TObject);
begin
 Ygl := Ygl - 5;
end;

procedure TForm1.Button6Click(Sender: TObject);
begin
  Zgl := Zgl + 5;
end;

procedure TForm1.Button7Click(Sender: TObject);
begin
  Zgl := Zgl - 5;
end;

procedure TForm1.Button8Click(Sender: TObject);
begin
  Br := Br + 0.01;
end;

procedure TForm1.Button9Click(Sender: TObject);
begin
  Br := Br - 0.01;
end;

procedure TForm1.Button10Click(Sender: TObject);
var
   s1, s2, s3, s4, s5, tmpSS : String;
begin
  s1 := form1.Edit5.text;
  if s1 = '' then s1 := '0';
  s2 := form1.Edit1.text;
  if s2 = '' then s1 := '0';
  s3 := form1.Edit3.text;
  if s3 = '' then s1 := '0';
  s4 := form1.Edit4.text;
  if s4 = '' then s1 := '0';
  s5 := form1.Edit6.text;
  if s5 = '' then s1 := '0';

  //GenGkode ('bolgarka.txt', 'G00 G49 G40.1 G17 G80 G50 G90', 'G0,G1', strToFloat(form1.Edit5.text), strToFloat(form1.Edit1.text), strToint(form1.Edit3.text), strToint(form1.Edit4.text), strToint(form1.Edit6.text));
  tmpSS := copy(FileNm, 1, length (FileNm) - 4)  + '.nc';
  GenGkode (tmpSS, 'G00 G49 G40.1 G17 G80 G50 G90', 'G0,G1', strToFloat(s1), strToFloat(s2), strToFloat(s3), strToFloat(s4), strToFloat(s5));
end;

procedure TForm1.Edit2Change(Sender: TObject);
begin
  if Edit2.Text = '' then Edit2.Text := '0';
  CreateGLlist;
end;

procedure TForm1.Edit1Change(Sender: TObject);
begin
  if Edit1.Text = '' then Edit1.Text := '0';
  CreateGLlist;
end;

procedure TForm1.Button11Click(Sender: TObject);
begin
  Mode := Mode * (-1);
  CreateGLlist;
  if Mode > 0 then Button11.Caption := 'Полигоны'
     else Button11.Caption := 'Линии';
end;

end.
