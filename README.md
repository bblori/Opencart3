I would like to add a variable/display in common/header twig file, which can be managed from an new extension. The new extension is created. starter_module

I added in: admin/view/template/extension/module/starter_module.twig


```
<div class="form-group">
        <label class="col-sm-2 control-label" for="input-new">New</label>
        <div class="col-sm-10">
        <select name="new" id="input-new" class="form-control">
            {% if new %}
            <option value="1" selected="selected">Enabled</option>
            <option value="0">Disabled</option>
            {% else %}
            <option value="1">Enabled</option>
            <option value="0" selected="selected">Disabled</option>
            {% endif %}
        </select>
        </div>
</div>
```

in admin/controller/extension/module/starter_module.php

```
if (isset($this->request->post['new'])) {
  $data['new'] = $this->request->post['new'];
} elseif (!empty($module_info)) {
  $data['new'] = $module_info['new'];
} else {
  $data['new'] = '';
}
```

in catalog/controller/extension/module/starter_module.php

```
$data['new'] = $this->config->get('new');

$data['new'] = (int) $setting['new'];  

```

in catalog/view/theme/default/template/common/header.twig

```
{% if new %}Enabled {% else %} disabled{% endif %}
```

But always I got the result only disabled, what is missing? cannot be sent variable from extension to common header?

Please, help me if you know the issue.

You can see here one of my working variable which was set in setting/setting files and is working.

https://github.com/bblori/Enable-Style-OC3

Solution I created a starter-module.xml in vqmod/xml folder.

```
<modification>
    <name>Starter Module</name>
    <code>starter-module</code>
    <version>3.0.1</version>
    <author>Denise (rei7092@gmail.com)</author>
    <link>http://demo.j-mall.com.tw/</link>
		<file path="catalog/controller/common/header.php">
			<operation>
				<search><![CDATA[return $this->load->view('common/header', $data);]]></search>
				<add position="before"><![CDATA[
	                    $data['config_new'] = $this->config->get('config_new');	
						$data['config_boxed'] = $this->config->get('config_boxed');
						$data['config_szinek'] = $this->config->get('config_szinek');
						$data['config_hatterszin'] = $this->config->get('config_hatterszin');
	           ]]></add>
	        </operation>
	    </file>
	    <file path="catalog/controller/common/header.php">
	    <operation>
				<search><![CDATA[$data['newsletter'] = $this->url->link('account/newsletter', '', true);]]></search>
				<add position="after"><![CDATA[
					if (is_file(DIR_IMAGE . $this->config->get('config_headbg'))) {
							$data['headbg'] = $server . 'image/' . $this->config->get('config_headbg');
						} else {
							$data['headbg'] = '';
						}
						]]></add>
	        </operation>
	    </file>
</modification>

```

Many thanks for Denise Die.