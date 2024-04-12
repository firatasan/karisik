```abap

TYPES: BEGIN OF typ_kunnr,
         kunnr TYPE kunnr,
       END OF typ_kunnr.

DATA lt_kunnr TYPE TABLE OF typ_kunnr.

lt_kunnr = VALUE #( ( kunnr = '0000010000' ) ( kunnr = '0000010001' ) ( kunnr = '0000010002' ) ).

" 1 - FOR ALL ENTRIES
SELECT * FROM kna1
  INTO TABLE @DATA(lt_data)
  FOR ALL ENTRIES IN @lt_kunnr
  WHERE kunnr = @lt_kunnr-kunnr.

cl_salv_table=>factory(
  IMPORTING
    r_salv_table   =   DATA(lo_salv)                        " Basisklasse einfache ALV Tabellen
  CHANGING
    t_table        = lt_data
).

lo_salv->display( ).


" 2 - RANGE
DATA lr_kunnr TYPE RANGE OF kunnr.

lr_kunnr = VALUE #( FOR ls_kunnr IN lt_kunnr ( sign = 'I' option = 'EQ' low = ls_kunnr-kunnr ) ).

SELECT * FROM kna1
  INTO TABLE @lt_data
  WHERE kunnr IN @lr_kunnr.







not CDS lerde for all entries olmuyor range yapmak lazim

    REFRESH mt_data.

*    SELECT * FROM ABC
*      INTO CORRESPONDING FIELDS OF TABLE @mt_data
*      FOR ALL ENTRIES IN @mt_cs_order
*      WHERE cs_order = @mt_cs_order-cs_order.

    DATA lr_cs_order TYPE RANGE OF aufnr.


```

    lr_cs_order = VALUE #( FOR ls_cs_order IN mt_cs_order ( sign = 'I' option = 'EQ' low = ls_cs_order-cs_order ) ).

    SELECT * FROM ABC
      INTO CORRESPONDING FIELDS OF TABLE @mt_data
      WHERE cs_order IN @lr_cs_order.
